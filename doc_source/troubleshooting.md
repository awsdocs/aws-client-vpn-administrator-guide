# Troubleshooting Client VPN<a name="troubleshooting"></a>

The following topic can help you troubleshoot problems that you might have with a Client VPN endpoint\.

For more information about troubleshooting OpenVPN\-based software that clients use to connect to a Client VPN, see [Troubleshooting Your Client VPN Connection](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/troubleshooting.html) in the *AWS Client VPN User Guide*\.

**Topics**
+ [Unable to Resolve Client VPN Endpoint DNS Name](#resolve-host-name)
+ [Traffic Is Not Being Split between Subnets](#split-traffic)
+ [Authorization Rules for Active Directory Groups Not Working as Expected](#ad-group-auth-rules)
+ [Clients Can't Access a Peered VPC, Amazon S3, or the Internet](#no-internat-access)
+ [Access to a Peered VPC, Amazon S3, or the Internet Is Intermittent](#intermittent-access)
+ [Client Software Returns TLS Error](#client-cannot-connect)
+ [Client Software Returns User Name and Password Errors](#client-user-name-password-mfa)

## Unable to Resolve Client VPN Endpoint DNS Name<a name="resolve-host-name"></a>

**Problem**  
I am unable to resolve the Client VPN endpoint's DNS name\.

**Cause**  
The Client VPN endpoint configuration file includes a parameter called `remote-random-hostname`\. This parameter forces the client to prepend a random string to the DNS name to prevent DNS caching\. Some clients do not recognize this parameter and therefore, they do not prepend the required random string to the DNS name\.

**Solution**  
Open the Client VPN endpoint configuration file using your preferred text editor\. Locate the line that specifies the Client VPN endpoint DNS name, and prepend a random string to it so that the format is *random\_string\.displayed\_DNS\_name*\. For example:
+ Original DNS name: `cvpn-endpoint-0102bc4c2eEXAMPLE.clientvpn.us-west-2.amazonaws.com`
+ Modified DNS name: `asdfa.cvpn-endpoint-0102bc4c2eEXAMPLE.clientvpn.us-west-2.amazonaws.com`

## Traffic Is Not Being Split between Subnets<a name="split-traffic"></a>

**Problem**  
I am trying to split network traffic between two subnets\. Private traffic should be routed through a private subnet, while internet traffic should be routed through a public subnet\. However, only one route is being used even though I have added both routes to the Client VPN endpoint route table\.

**Cause**  
You can associate multiple subnets with a Client VPN endpoint, but you can associate only one subnet per Availability Zone\. The purpose of multiple subnet association is to provide high availability and Availability Zone redundancy for clients\. However, Client VPN does not enable you to selectively split traffic between the subnets that are associated with the Client VPN endpoint\.

Clients connect to a Client VPN endpoint based on the DNS round\-robin algorithm\. This means that their traffic can be routed through any of the associated subnets when they establish a connection\. Therefore, they might experience connectivity issues if they land on an associated subnet that does not have the required route entries\.

For example, say that you configure the following subnet associations and routes:
+ Subnet associations
  + Association 1: Subnet\-A \(us\-east\-1a\)
  + Association 2: Subnet\-B \(us\-east\-1b\)
+ Routes
  + Route 1: 10\.0\.0\.0/16 routed to Subnet\-A
  + Route 2: 172\.31\.0\.0/16 routed to Subnet\-B

In this example, clients that land on Subnet\-A when they connect cannot access Route 2, while clients that land on Subnet\-B when they connect cannot access Route 1\.

**Solution**  
Verify that the Client VPN endpoint has the same route entries with targets for each associated network\. This ensures that clients have access to all routes regardless of the subnet through which their traffic is routed\.

## Authorization Rules for Active Directory Groups Not Working as Expected<a name="ad-group-auth-rules"></a>

**Problem**  
I have configured authorization rules for my Active Directory groups, but they are not working as I expected\. I have added an authorization rule for `0.0.0.0/0` to authorize traffic for all networks, but traffic still fails for specific destination CIDRs\.

**Cause**  
Authorization rules are indexed on network CIDRs\. Authorization rules must grant Active Directory groups access to specific network CIDRs\. Authorization rules for `0.0.0.0/0` are handled as a special case, and are therefore evaluated last, regardless of the order in which the authorization rules are created\.

For example, say that you create three authorization rules in the following order:
+ Rule 1: Group 1 access to `10.1.0.0/16`
+ Rule 2: Group 1, Group 2, and Group 3 access to `0.0.0.0/0`
+ Rule 3: Group 2 access to `172.131.0.0/16`

In this example, Rule 2 is evaluated last\. Group 1 has access to `10.1.0.0/16` only, and Group 2 has access to `172.131.0.0/16` only\. Group 3 does not have access to `10.1.0.0/16` or `172.131.0.0/16`, but it has access to all other networks\. If you remove Rules 1 and 3, all three groups have access to all networks\.

In addition, Client VPN uses longest prefix matching when evaluating authorization rules\.

**Solution**  
Verify that you create authorization rules that explicitly grant Active Directory groups access to specific network CIDRs\. If you add an authorization rule for `0.0.0.0/0`, keep in mind that it will be evaluated last, and that previous authorization rules may limit the networks to which it grants access\.

## Clients Can't Access a Peered VPC, Amazon S3, or the Internet<a name="no-internat-access"></a>

**Problem**  
I have properly configured my Client VPN endpoint routes, but my clients can't access a peered VPC, Amazon S3, or the internet\. 

**Solution**  
The following flow chart contains the steps to diagnose internet, peered VPC, and Amazon S3 connectivity issues\.

![\[Client VPN troubleshooting steps\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/cvpn-flow.png)

1. For access to the internet, add an authorization rule for `0.0.0.0/0`\.

   For access to a peered VPC, add an authorization rule for the IPv4 CIDR range of the VPC\.

   For access to S3, specify the IP address of the Amazon S3 endpoint\.

1. Check whether you are able to resolve the DNS name\.

   If you are unable to resolve the DNS name, verify that you have specified the DNS servers for the Client VPN endpoint\. If you manage your own DNS server, specify its IP address\. Verify that the DNS server is accessible from the VPC\. 

   If you're unsure about which IP address to use, use *VPC DNS resolver \.2 IP*\.

1. Check whether you are able to ping an IP address\. If you do not get a response, make sure that the route table for the associated subnets has a default route that targets either an internet gateway or a NAT gateway\. If the default route is in place, verify that the associated subnet does not have network access control list rules that block inbound and outbound traffic\.

   If you are unable to reach a peered VPC, verify that the associated subnet's route table has a route entry for the peered VPC\.

   If you are unable to reach Amazon S3, verify that the associated subnet's route table has a route entry for the gateway VPC endpoint\.

1. Check whether you can ping a public IP address with a payload larger than 1400 bytes\. Use one of the following commands:
   + Windows

     ```
     C:\> ping  8.8.8.8 -l 1480 -f
     ```
   + Linux

     ```
     $ ping -s 1480 8.8.8.8 -M do
     ```

   If you cannot ping an IP address with a payload larger than 1400 bytes, open the Client VPN endpoint `.ovpn` configuration file using your preferred text editor, and add the following\.

   ```
   mssfix 1328
   ```

## Access to a Peered VPC, Amazon S3, or the Internet Is Intermittent<a name="intermittent-access"></a>

**Problem**  
I have intermittent connectivity issues when connecting to a peered VPC, Amazon S3, or the internet, but access to associated subnets is unaffected\. I need to disconnect and reconnect in order to resolve the connectivity issues\.

**Cause**  
Clients connect to a Client VPN endpoint based on the DNS round\-robin algorithm\. This means that their traffic can be routed through any of the associated subnets when they establish a connection\. Therefore, they might experience connectivity issues if they land on an associated subnet that does not have the required route entries\.

**Solution**  
Verify that the Client VPN endpoint has the same route entries with targets for each associated network\. This ensures that clients have access to all routes regardless of the associated subnet through which their traffic is routed\.

For example, say that your Client VPN endpoint has three associated subnets \(Subnet A, B, and C\), and you want to enable internet access for your clients\. To do this, you must add three `0.0.0.0/0` routes \- one that targets each associated subnet:
+ Route 1: `0.0.0.0/0` for Subnet A
+ Route 2: `0.0.0.0/0` for Subnet B
+ Route 3: `0.0.0.0/0` for Subnet C

## Client Software Returns TLS Error<a name="client-cannot-connect"></a>

**Problem**  
I used to be able to connect my clients to the Client VPN successfully, but now the OpenVPN\-based client returns the following error when it tries to connect: 

```
TLS Error: TLS key negotiation failed to occur within 60 seconds (check your network connectivity) 
TLS Error: TLS handshake failed
```

**Possible Causes**  
If you use mutual authentication and you imported a client certificate revocation list, the client certificate revocation list might have expired\. During the authentication phase, the Client VPN endpoint checks the client certificate against the client certificate revocation list that you imported\. If the client certificate revocation list has expired, you cannot connect to the Client VPN endpoint\.

Alternatively, there might be an issue with the OpenVPN\-based software that the client is using to connect to the Client VPN\.

**Solution**  
Check the expiry date of your client certificate revocation list by using the OpenSSL tool\. 

```
$ openssl crl -in path_to_crl_pem_file -noout -nextupdate
```

The output displays the expiry date and time\. If the client certificate revocation list has expired, you must create a new one and import it to the Client VPN endpoint\. For more information, see [Client Certificate Revocation Lists](cvpn-working-certificates.md)\.

For more information about troubleshooting OpenVPN\-based software, see [Troubleshooting Your Client VPN Connection](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/troubleshooting.html) in the *AWS Client VPN User Guide*\.

## Client Software Returns User Name and Password Errors<a name="client-user-name-password-mfa"></a>

**Problem**  
I use Active Directory authentication for my Client VPN endpoint and I used to be able to connect my clients to the Client VPN successfully\. But now, clients are getting invalid user name and password errors\.

**Possible Causes**  
If you use Active Directory authentication and if you enabled multi\-factor authentication \(MFA\) after you distributed the client configuration file, the file does not contain the necessary information to prompt users to enter their MFA code\. Users are prompted to enter their user name and password only, and authentication fails\.

**Solution**  
Download a new client configuration file and distribute it to your clients\. Verify that the new file contains the following line\.

```
static-challenge "Enter MFA code " 1
```

For more information, see [Export Client Configuration](cvpn-working-endpoints.md#cvpn-working-endpoint-export)\. Test the MFA configuration for your Active Directory without using the Client VPN endpoint to verify that MFA is working as expected\.