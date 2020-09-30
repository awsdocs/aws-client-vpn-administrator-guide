# Client VPN endpoints<a name="cvpn-working-endpoints"></a>

All client VPN sessions terminate at the Client VPN endpoint\. You configure the Client VPN endpoint to manage and control all client VPN sessions\. 

**Topics**
+ [Create a Client VPN endpoint](#cvpn-working-endpoint-create)
+ [Modify a Client VPN endpoint](#cvpn-working-endpoint-modify)
+ [Export and configure the client configuration file](#cvpn-working-endpoint-export)
+ [View Client VPN endpoints](#cvpn-working-endpoint-view)
+ [Delete a Client VPN endpoint](#cvpn-working-endpoint-delete)

## Create a Client VPN endpoint<a name="cvpn-working-endpoint-create"></a>

Create a Client VPN endpoint to enable your clients to establish a VPN session\.

The Client VPN must be created in the same AWS account in which the intended target network is provisioned\.

**Prerequisites**  
Before you begin, ensure that you do the following:
+ Review the rules and limitations in [Limitations and rules of Client VPN](what-is.md#what-is-limitations)\.
+ Generate the server certificate, and if required, the client certificate\. For more information, see [Authentication](client-authentication.md)\.

**To create a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints** and then choose **Create Client VPN Endpoint\.**

1. \(Optional\) For **Description**, enter a brief description for the Client VPN endpoint\.

1. For **Client IPv4 CIDR**, specify an IP address range, in CIDR notation, from which to assign client IP addresses\.

1. For **Server certificate ARN**, specify the ARN for the TLS certificate to be used by the server\. Clients use the server certificate to authenticate the Client VPN endpoint to which they are connecting\.
**Note**  
The server certificate must be provisioned in AWS Certificate Manager \(ACM\)\.

1. Specify the authentication method to be used to authenticate clients when they establish a VPN connection\. You must select at least one authentication method\.
   + To use user\-based authentication, select **Use user\-based authentication**, and then choose one of the following:
     + **Active Directory authentication**: Choose this option for Active Directory authentication\. For **Directory ID**, specify the ID of the Active Directory to use\.
     + **Federated authentication**: Choose this option for SAML\-based federated authentication\. For **SAML provider ARN**, specify the ARN of the IAM SAML identity provider\.
   + To use mutual certificate authentication, select **Use mutual authentication**, and then for **Client certificate ARN**, specify the ARN of the client certificate that's provisioned in AWS Certificate Manager \(ACM\)\.
**Note**  
If the client certificate has been issued by the same Certificate Authority \(Issuer\) as the server certificate, you can continue to use the server certificate ARN for the client certificate ARN\. If you've generated a separate client certificate and key for each user using the same CA as the server certificate, you can use the server certificate ARN\.

1. Specify whether to log data about client connections using Amazon CloudWatch Logs\. For **Do you want to log the details on client connections?**, do one of the following:
   + To enable client connection logging, choose **Yes**\. For **CloudWatch Logs log group name**, enter the name of the log group to use\. For **CloudWatch Logs log stream name**, enter the name of the log stream to use, or leave this option blank to let us create a log stream for you\.
   + To disable client connection logging, choose **No**\.

1. \(Optional\) Specify which DNS servers to use for DNS resolution\. To use custom DNS servers, for **DNS Server 1 IP address** and **DNS Server 2 IP address**, specify the IP addresses of the DNS servers to use\. To use VPC DNS server, for either **DNS Server 1 IP address** or **DNS Server 2 IP address**, specify the IP addresses, and add the VPC DNS server IP address\.
**Note**  
Verify that the DNS servers can be reached by clients\.

1. \(Optional\) To have the endpoint be a split\-tunnel VPN endpoint, select **Enable split\-tunnel**\.

   By default, split\-tunnel on a VPN endpoint is disabled\.

1. \(Optional\) By default, the Client VPN server uses the `UDP` transport protocol\. To use the `TCP` transport protocol instead, for **Transport Protocol**, select **TCP**\.
**Note**  
UDP typically offers better performance than TCP\. You cannot change the transport protocol after you create the Client VPN endpoint\.

1. \(Optional\) For **VPC ID**, choose the VPC to associate with the Client VPN endpoint\. For **Security Group IDs**, choose one or more of the VPC's security groups to apply to the Client VPN endpoint\.

1. \(Optional\) For **VPN port**, choose the VPN port number\. The default is 443\.

1. Choose **Create Client VPN Endpoint**\.

After you create the Client VPN endpoint, do the following to complete the configuration and enable clients to connect:
+ The initial state of the Client VPN endpoint is `pending-associate`\. Clients can only connect to the Client VPN endpoint after you associate the first [target network](cvpn-working-target.md#cvpn-working-target-associate)\.
+ Create an [authorization rule](cvpn-working-rules.md) to specify which clients have access to the network\.
+ Download and prepare the Client VPN endpoint [configuration file](#cvpn-working-endpoint-export) to distribute to your clients\.
+ Instruct your clients to use the AWS\-provided client or another OpenVPN\-based client application to connect to the Client VPN endpoint\. For more information, see the [AWS Client VPN User Guide](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/)\.

**To create a Client VPN endpoint \(AWS CLI\)**  
Use the [create\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-client-vpn-endpoint.html) command\.

## Modify a Client VPN endpoint<a name="cvpn-working-endpoint-modify"></a>

After a Client VPN has been created, you can modify any of the following settings: 
+ The description
+ The server certificate
+ The client connection logging options
+ The DNS servers
+ The split\-tunnel option
+ The VPC and security group associations
+ The VPN port number

You cannot modify the client IPv4 CIDR range, authentication options, or transport protocol after the Client VPN endpoint has been created\.

You can modify a Client VPN endpoint by using the console or the AWS CLI\. 

**To modify a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to modify, choose **Actions**, and then choose **Modify Client VPN Endpoint**\.

1. Make the required changes and choose **Modify Client VPN Endpoint**\.

**To modify a Client VPN endpoint \(AWS CLI\)**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command\.

## Export and configure the client configuration file<a name="cvpn-working-endpoint-export"></a>

The Client VPN endpoint configuration file is the file that clients \(users\) use to establish a VPN connection with the Client VPN endpoint\. You must download \(export\) this file and distribute it to all clients who need access to the VPN\.

If your Client VPN endpoint uses mutual authentication, you must [add the client certificate and the client private key to the \.ovpn configuration file](#add-config-file-cert-key) that you download\. After you add the information, clients can import the \.ovpn file into the OpenVPN client software\.

**Important**  
If you do not add the client certificate and the client private key information to the file, clients that authenticate using mutual authentication cannot connect to the Client VPN endpoint\.

By default, the “\-\-remote\-random\-hostname” option in the OpenVPN client configuration enables wildcard DNS\. Because wildcard DNS is enabled, the client does not cache the IP address of the endpoint and you will not be able to ping the DNS name of the endpoint\. 

If your Client VPN endpoint uses Active Directory authentication and if you enable multi\-factor authentication \(MFA\) on your directory after you distribute the client configuration file, you must download a new file and redistribute it to your clients\. Clients cannot use the previous configuration file to connect to the Client VPN endpoint\.

### Export the client configuration file<a name="export-client-config-file"></a>

You can export the client configuration by using the console or the AWS CLI\.

**To export client configuration \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint for which to download the client configuration and choose **Download Client Configuration**\.

**To export client configuration \(AWS CLI\)**  
Use the [export\-client\-vpn\-client\-configuration](https://docs.aws.amazon.com/cli/latest/reference/ec2/export-client-vpn-client-configuration.html) command and specify the output file name\.

```
$ aws ec2 export-client-vpn-client-configuration --client-vpn-endpoint-id endpoint_id --output text>config_filename.ovpn
```

### Add the client certificate and key information \(mutual authentication\)<a name="add-config-file-cert-key"></a>

If your Client VPN endpoint uses mutual authentication, you must add the client certificate and the client private key to the \.ovpn configuration file that you download\.

**To add the client certificate and key information \(mutual authentication\)**  
You can use one of the following options\.

\(Option 1\) Distribute the client certificate and key to clients along with the Client VPN endpoint configuration file\. In this case, specify the path to the certificate and key in the configuration file\. Open the configuration file using your preferred text editor, and add the following to the end of the file\. Replace */path/* with the location of the client certificate and key \(the location is relative to the client that's connecting to the endpoint\)\.

```
cert /path/client1.domain.tld.crt
key /path/client1.domain.tld.key
```

\(Option 2\) Add the contents of the client certificate between `<cert>``</cert>` tags and the contents of the private key between `<key>``</key>` tags to the configuration file\. If you choose this option, you distribute only the configuration file to your clients\.

If you generated separate client certificates and keys for each user that will connect to the Client VPN endpoint, repeat this step for each user\.

The following is an example of the format of a Client VPN configuration file that includes the client certificate and key\.

```
client
dev tun
proto udp
remote asdf.cvpn-endpoint-0011abcabcabcabc1.prod.clientvpn.eu-west-2.amazonaws.com 443
remote-random-hostname
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
cipher AES-256-GCM
verb 3

<ca>
Contents of CA
</ca>

<cert>
Contents of client certificate (.crt) file
</cert>

<key>
Contents of private key (.key) file
</key>

reneg-sec 0
```

## View Client VPN endpoints<a name="cvpn-working-endpoint-view"></a>

You can view information about Client VPN endpoints by using the console or the AWS CLI\.

**To view Client VPN endpoints using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to view\.

1. Use tabs to view the associated target networks, authorization rules, routes, and client connections\.

**To view Client VPN endpoints using the AWS CLI**  
Use the [describe\-client\-vpn\-endpoints](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-endpoints.html) command\.

## Delete a Client VPN endpoint<a name="cvpn-working-endpoint-delete"></a>

When you delete a Client VPN endpoint, its state is changed to `deleting` and clients can no longer connect to it\. You must disassociate all associated target networks before you can delete a Client VPN endpoint\.

You can delete a Client VPN endpoint by using the console or the AWS CLI\.

**To delete a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to delete, choose **Actions**, choose **Delete Client VPN Endpoint**, and then **Yes, Delete**\.

**To delete a Client VPN endpoint \(AWS CLI\)**  
Use the [delete\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-client-vpn-endpoint.html) command\.