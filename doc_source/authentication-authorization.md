# Client Authentication and Authorization<a name="authentication-authorization"></a>

Client VPN provides authentication and authorization capabilities\.

**Topics**
+ [Authentication](#client-authentication)
+ [Authorization](#client-authorization)

## Authentication<a name="client-authentication"></a>

Authentication is implemented at the first point of entry into the AWS Cloud\. It is used to determine whether clients are allowed to connect to the Client VPN endpoint\. If authentication succeeds, clients connect to the Client VPN endpoint and establish a VPN session\. If authentication fails, the connection is denied and the client is prevented from establishing a VPN session\.

Client VPN offers two types of client authentication: Active Directory authentication and mutual authentication\. You can choose to use either one or both authentication methods\.

### Active Directory Authentication<a name="ad"></a>

Client VPN provides Active Directory support by integrating with AWS Directory Service\. With Active Directory authentication, clients are authenticated against existing Active Directory groups\. Using AWS Directory Service, Client VPN can connect to existing Active Directories provisioned in AWS or in your on\-premises network\. This allows you to use your existing client authentication infrastructure\. If you are using an on\-premises Active Directory, you must configure an Active Directory Connector \(AD Connector\)\. You can use one Active Directory server to authenticate the users\. For more information about Active Directory integration, see the [AWS Directory Service Administration Guide](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

Client VPN supports multi\-factor authentication \(MFA\) when it's enabled for AWS Managed Microsoft AD or AD Connector\. If MFA is enabled, clients must enter a user name, password, and MFA code when they connect to a Client VPN endpoint\. For more information about enabling MFA, see [Enable Multi\-Factor Authentication for AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_mfa.html) and [Enable Multi\-Factor Authentication for AD Connector](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_mfa.html) in the *AWS Directory Service Administration Guide*\. 

### Mutual Authentication<a name="mutual"></a>

With mutual authentication, Client VPN uses certificates to perform authentication between the client and the server\. Certificates are a digital form of identification issued by a certificate authority \(CA\)\. The server uses client certificates to authenticate clients when they attempt to connect to the Client VPN endpoint\. The server and client certificates must be uploaded to AWS Certificate Manager \(ACM\)\. For more information about provisioning and uploading certificates in ACM, see the [AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)\. 

You only need to upload the client certificate to ACM when the Certificate Authority \(Issuer\) of the client certificate is different from the Certificate Authority \(Issuer\) of the server certificate\.

You can create a separate client certificate and key for each client that will connect to the Client VPN endpoint\. This enables you to revoke a specific client certificate if a user leaves your organization\.

A Client VPN endpoint supports 1024\-bit and 2048\-bit RSA key sizes only\.

The following procedure uses OpenVPN easy\-rsa to generate the server and client certificates and keys, and then uploads the server certificate and key to ACM\. For more information, see the [Easy\-RSA 3 Quickstart README](https://github.com/OpenVPN/easy-rsa/blob/v3.0.6/README.quickstart.md)\.

**To generate the server and client certificates and keys and upload them to ACM**

1. Clone the OpenVPN easy\-rsa repo to your local computer\.

   ```
   $ git clone https://github.com/OpenVPN/easy-rsa.git
   ```

1. Navigate into the `easy-rsa/easyrsa3` folder in your local repo\.

   ```
   $ cd easy-rsa/easyrsa3
   ```

1. Initialize a new PKI environment\.

   ```
   $ ./easyrsa init-pki
   ```

1. Build a new certificate authority \(CA\)\.

   ```
   $ ./easyrsa build-ca nopass
   ```

   Follow the prompts to build the CA\.

1. Generate the server certificate and key\.

   ```
   $ ./easyrsa build-server-full server nopass
   ```

1. Generate the client certificate and key\.

   Make sure to save the client certificate and the client private key because you will need them when you configure the client\.

   ```
   $ ./easyrsa build-client-full client1.domain.tld nopass
   ```

   You can optionally repeat this step for each client \(end user\) that requires a client certificate and key\.

1. Copy the server certificate and key and the client certificate and key to a custom folder and then navigate into the custom folder\.

   Before you copy the certificates and keys, create the custom folder by using the `mkdir` command\. The following example creates a custom folder in your home directory\.

   ```
   $ mkdir ~/custom_folder/
   $ cp pki/ca.crt ~/custom_folder/
   $ cp pki/issued/server.crt ~/custom_folder/
   $ cp pki/private/server.key ~/custom_folder/
   $ cp pki/issued/client1.domain.tld.crt ~/custom_folder
   $ cp pki/private/client1.domain.tld.key ~/custom_folder/
   $ cd ~/custom_folder/
   ```

1. Upload the server certificate and key to ACM\.

   ```
   $ aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt --region region
   ```
**Note**  
Be sure to upload the certificate and key in the same Region in which you intend to create the Client VPN endpoint\.

1. Upload the client certificate and key to ACM\.

   ```
   $ aws acm import-certificate --certificate fileb://client1.domain.tld.crt --private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt --region region
   ```
**Note**  
Be sure to upload the certificate and key in the same Region in which you intend to create the Client VPN endpoint\.

## Authorization<a name="client-authorization"></a>

Client VPN supports two types of authorization: security groups and network\-based authorization \(using authorization rules\)\.

### Security Groups<a name="security-groups"></a>

Client VPN automatically integrates with security groups\. When you associate a subnet with a Client VPN endpoint, we automatically apply the VPC's default security group\. You can change the security group after you associate the first target network\. You can enable Client VPN users to access your applications in a VPC, by adding a rule to allow traffic from the security group that was applied to the association\. Conversely, you can restrict access for Client VPN users, by not specifying the security group that was applied to the association\. For more information, see [Apply a Security Group to a Target Network](cvpn-working-target.md#cvpn-working-target-apply)\. The security group rules you require might also depend on the kind of VPN access you want to configure\. For more information, see [Scenarios and Examples](scenario.md)\.

### Network\-based Authorization<a name="auth-rules"></a>

Network\-based authorization is implemented using authorization rules\. For each network that you want to enable access, you must configure authorization rules that limits the users who have access\. For a specified network, you configure the Active Directory group that is allowed access\. Only users who belong to the specified Active Directory group can access the specified network\. If you are not using Active Directory, or you want to open access to all users, you can specify a rule that grants access to all clients\. For more information, see [Authorization Rules](cvpn-working-rules.md)\.
