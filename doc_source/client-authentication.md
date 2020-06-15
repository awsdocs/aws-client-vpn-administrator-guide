# Authentication<a name="client-authentication"></a>

Authentication is implemented at the first point of entry into the AWS Cloud\. It is used to determine whether clients are allowed to connect to the Client VPN endpoint\. If authentication succeeds, clients connect to the Client VPN endpoint and establish a VPN session\. If authentication fails, the connection is denied and the client is prevented from establishing a VPN session\.

Client VPN offers the following types of client authentication: 
+ [Active Directory authentication](#ad) \(user\-based\)
+ [Mutual authentication](#mutual) \(certificate\-based\)
+ [Single sign\-on \(SAML\-based federated authentication\)](#federated-authentication) \(user\-based\)

You can use one or a combination of the following:
+ Mutual authentication and federated authentication
+ Mutual authentication and Active Directory authentication

**Important**  
To create a Client VPN endpoint, you must provision a server certificate in AWS Certificate Manager, regardless of the type of authentication you use\. For more information about creating and provisioning a server certificate, see the steps in [Mutual authentication](#mutual)\.

## Active Directory authentication<a name="ad"></a>

Client VPN provides Active Directory support by integrating with AWS Directory Service\. With Active Directory authentication, clients are authenticated against existing Active Directory groups\. Using AWS Directory Service, Client VPN can connect to existing Active Directories provisioned in AWS or in your on\-premises network\. This allows you to use your existing client authentication infrastructure\. If you are using an on\-premises Active Directory and you do not have an existing AWS Managed Microsoft AD, you must configure an Active Directory Connector \(AD Connector\)\. You can use one Active Directory server to authenticate the users\. For more information about Active Directory integration, see the [AWS Directory Service Administration Guide](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

Client VPN supports multi\-factor authentication \(MFA\) when it's enabled for AWS Managed Microsoft AD or AD Connector\. If MFA is enabled, clients must enter a user name, password, and MFA code when they connect to a Client VPN endpoint\. For more information about enabling MFA, see [Enable Multi\-Factor Authentication for AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_mfa.html) and [Enable Multi\-Factor Authentication for AD Connector](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_mfa.html) in the *AWS Directory Service Administration Guide*\. 

## Mutual authentication<a name="mutual"></a>

With mutual authentication, Client VPN uses certificates to perform authentication between the client and the server\. Certificates are a digital form of identification issued by a certificate authority \(CA\)\. The server uses client certificates to authenticate clients when they attempt to connect to the Client VPN endpoint\. The server and client certificates must be uploaded to AWS Certificate Manager \(ACM\)\. For more information about provisioning and uploading certificates in ACM, see the [AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)\. 

You only need to upload the client certificate to ACM when the Certificate Authority \(Issuer\) of the client certificate is different from the Certificate Authority \(Issuer\) of the server certificate\.

You can create a separate client certificate and key for each client that will connect to the Client VPN endpoint\. This enables you to revoke a specific client certificate if a user leaves your organization\.

A Client VPN endpoint supports 1024\-bit and 2048\-bit RSA key sizes only\.

------
#### [ Linux/macOS ]

The following procedure uses OpenVPN easy\-rsa to generate the server and client certificates and keys, and then uploads the server certificate and key to ACM\. For more information, see the [Easy\-RSA 3 Quickstart README](https://github.com/OpenVPN/easy-rsa/blob/v3.0.6/README.quickstart.md)\.

**To generate the server and client certificates and keys and upload them to ACM**

1. Clone the OpenVPN easy\-rsa repo to your local computer and navigate to the `easy-rsa/easyrsa3` folder\.

   ```
   $ git clone https://github.com/OpenVPN/easy-rsa.git
   ```

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

1. Upload the server certificate and key and the client certificate and key to ACM\. The following commands use the AWS CLI\.

   ```
   $ aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt --region region
   ```

   ```
   $ aws acm import-certificate --certificate fileb://client1.domain.tld.crt --private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt --region region
   ```

   To upload the certificates using the ACM console, see [Import a Certificate](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-api-cli.html) in the *AWS Certificate Manager User Guide*\.
**Note**  
Be sure to upload the certificates and keys in the same Region in which you intend to create the Client VPN endpoint\.  
You only need to upload the client certificate to ACM when the CA of the client certificate is different from the CA of the server certificate\. In the steps above, the client certificate uses the same CA as the server certificate, however, the steps to upload the client certificate are included here for completeness\.

------
#### [ Windows ]

The following procedure installs the OpenVPN software, and then uses it to generate the server and client certificates and keys\.

**To generate the server and client certificates and keys and upload them to ACM**

1. Go to the [OpenVPN Community Downloads](https://openvpn.net/community-downloads/) page and download the Windows installer for your version of Windows\.

1. Run the installer\. On the first page of the OpenVPN Setup Wizard, choose **Next**\.

1. On the **License Agreement** page, choose **I Agree**\.

1. On the **Choose Components** page, choose **EasyRSA 2 Certificate Management Scripts**\. Choose **Next**, and then choose **Install**\.

1. Choose **Next**, and then **Finish** to complete the installation\.

1. Open the command prompt as an Administrator, navigate to the OpenVPN directory, and run `init-config`\.

   ```
   C:\> cd \Program Files\OpenVPN\easy-rsa
   ```

   ```
   C:\> init-config
   ```

1. Open the `vars.bat` file using Notepad\.

   ```
   C:\> notepad vars.bat
   ```

1. In the file, do the following and save your changes\.
   + For `set KEY_SIZE`, change the value to `2048`\.
   + Provide values for the following parameters\. Do not leave any of the values blank\.
     + KEY\_COUNTRY
     + KEY\_PROVINCE
     + KEY\_CITY
     + KEY\_ORG
     + KEY\_EMAIL

1. In the command line, run the `vars.bat` file and then run `clean-all`\.

   ```
   C:\> vars
   ```

   ```
   C:\> clean-all
   ```

1. Build a new certificate authority \(CA\)\.

   ```
   C:\> build-ca
   ```

   Follow the prompts to build the CA\. You can leave the default values for all of the fields\. If you prefer, you can change the Common Name to the server's domain name, for example, `server.example.com`\.

1. Generate the server certificate and key\.

   ```
   C:\> build-key-server server
   ```

   Follow the prompts to generate the certificate and key\. You can leave the default values for all of the fields, except Common Name\. For this field, you must specify a server domain in a domain name format\. For example, `server.example.com`\.

   When prompted to sign the certificate, enter `y` for both prompts\.

1. Generate the client certificate and key\.

   ```
   C:\> build-key client
   ```

   Follow the prompts to generate the certificate and key\. You can leave the default values for all of the fields, except Common Name\. For this field, you must specify a client domain in a domain name format\. For example, `client.example.com`\.

   When prompted to sign the certificate, enter `y` for both prompts\.

   You can optionally repeat this step for each client \(end user\) that requires a client certificate and key\.

1. Upload the server certificate and key and the client certificate and key to ACM\. The following commands use the AWS CLI\.

   ```
   C:\> aws acm import-certificate --certificate fileb://"C:\Program Files\OpenVPN\easy-rsa\keys\server.crt" --private-key fileb://"C:\Program Files\OpenVPN\easy-rsa\keys\server.key" --certificate-chain fileb://"C:\Program Files\OpenVPN\easy-rsa\keys\ca.crt" --region region
   ```

   ```
   C:\> aws acm import-certificate --certificate fileb://"C:\Program Files\OpenVPN\easy-rsa\keys\client.crt" --private-key fileb://"C:\Program Files\OpenVPN\easy-rsa\keys\client.key" --certificate-chain fileb://"C:\Program Files\OpenVPN\easy-rsa\keys\ca.crt"  --region region
   ```

   To upload the certificates using the ACM console, see [Import a Certificate](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-api-cli.html) in the *AWS Certificate Manager User Guide*\.
**Note**  
Be sure to upload the certificates and keys in the same Region in which you intend to create the Client VPN endpoint\.  
You only need to upload the client certificate to ACM when the CA of the client certificate is different from the CA of the server certificate\. In the steps above, the client certificate uses the same CA as the server certificate, however, the steps to upload the client certificate are included here for completeness\.

------

## Single sign\-on \(SAML 2\.0\-based federated authentication\)<a name="federated-authentication"></a>

AWS Client VPN supports identity federation with Security Assertion Markup Language 2\.0 \(SAML 2\.0\) for Client VPN endpoints\. You can use identity providers \(IdPs\) that support SAML 2\.0 to create centralized user identities\. You can then configure a Client VPN endpoint to use SAML\-based federated authentication, and associate it with the IdP\. Users then connect to the Client VPN endpoint using their centralized credentials\.

To enable your SAML\-based IdP to work with a Client VPN endpoint, you must do the following\.

1. Create a SAML\-based app in your chosen IdP to use with AWS Client VPN, or use an existing app\.

1. Configure your IdP to establish a trust relationship with AWS\. For resources, see [SAML\-based IdP configuration resources](#saml-config-resources)\.

1. In your IdP, generate and download a federation metadata document that describes your organization as an IdP\. This signed XML document is used to establish the trust relationship between AWS and the IdP\.

1. Create an IAM SAML identity provider in the same AWS account as the Client VPN endpoint\. The IAM SAML identity provider defines your organization's IdP\-to\-AWS trust relationship using the metadata document generated by the IdP\. For more information, see [Creating IAM SAML Identity Providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html) in the *IAM User Guide*\. If you later update the app configuration in the IdP, generate a new metadata document and update your IAM SAML identity provider\.
**Note**  
You do not need to create an IAM role to use the IAM SAML identity provider\.

1. Create a Client VPN endpoint\. Specify federated authentication as the authentication type, and specify the IAM SAML identity provider that you created\. For more information, see [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Export the [client configuration file](cvpn-working-endpoints.md#cvpn-working-endpoint-export) and distribute it to your users\. Instruct your users to download the latest version of the [AWS\-provided client](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/connect-aws-client-vpn-connect.html), and to use it to load the configuration file and connect to the Client VPN endpoint\.

### Authentication workflow<a name="federated-authentication-workflow"></a>

The following diagram provides an overview of the authentication workflow for a Client VPN endpoint that uses SAML\-based federated authentication\. When you create and configure the Client VPN endpoint, you specify the IAM SAML identity provider\.

![\[Authentication workflow\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/federated-auth-workflow.png)

1. The user opens the AWS\-provided client on their device and initiates a connection to the Client VPN endpoint\.

1. The Client VPN endpoint sends an IdP URL and authentication request back to the client, based on the information that was provided in the IAM SAML identity provider\.

1. The AWS\-provided client opens a new browser window on the user's device\. The browser makes a request to the IdP and displays a login page\.

1. The user enters their credentials on the login page, and the IdP sends a signed SAML assertion back to the client\.

1. The AWS\-provided client sends the SAML assertion to the Client VPN endpoint\.

1. The Client VPN endpoint validates the assertion and either allows or denies access to the user\.

### Requirements and considerations for SAML\-based federated authentication<a name="saml-requirements"></a>

The following are the requirements and considerations for SAML\-based federated authentication\.
+ When you configure users and groups using a SAML\-based IdP, the following rules apply:
  + Users can belong to a maximum of 25 groups\. We ignore any groups after the 25th group\.
  + The maximum length for the group ID is 255 characters\.
  + The maximum length for the name ID is 255 characters\. We truncate characters after the 255th character\.
+ SAML responses and SAML assertions must be signed and unencrypted\.
+ The maximum supported size for SAML responses is 128 KB\.
+ AWS Client VPN does not provide signed authentication requests\.
+ SAML single logout is not supported\. Users can log out by disconnecting from the AWS\-provided client, or you can [terminate the connections](cvpn-working-connections.md#cvpn-working-connections-disassociate)\.
+ A Client VPN endpoint supports a single IdP only\.
+ Multi\-factor authentication \(MFA\) is supported when it's enabled in your IdP\.
+ Users must use the AWS\-provided client to connect to the Client VPN endpoint\. They must use version 1\.2\.0 or later\. For more information, see [Connect using the AWS\-provided client](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/connect-aws-client-vpn-connect.html)\.
+ The following browsers are supported for IdP authentication: Apple Safari, Google Chrome, Microsoft Edge, and Mozilla Firefox\.
+ The AWS\-provided client reserves TCP port 35001 on users' devices for the SAML response\.
+ If the metadata document for the IAM SAML identity provider is updated with an incorrect or malicious URL, this can cause authentication issues for users, or result in phishing attacks\. Therefore, we recommend that you use AWS CloudTrail to monitor updates that are made to the IAM SAML identity provider\. For more information, see [Logging IAM and AWS STS calls with AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html) in the *IAM User Guide*\.
+ AWS Client VPN sends an AuthN request to the IdP via an HTTP Redirect binding\. Therefore, the IdP should support HTTP Redirect binding and it should be present in the IdP's metadata document\.
+ For the SAML assertion, you must use an email address format for the `NameID` attribute\.

### SAML\-based IdP configuration resources<a name="saml-config-resources"></a>

The following table lists the SAML\-based IdPs that we have tested for use with AWS Client VPN, and resources that can help you configure the IdP\.


| IdP | Resource | 
| --- | --- | 
| Okta | [Authenticate AWS Client VPN users with SAML](https://aws.amazon.com/blogs/networking-and-content-delivery/authenticate-aws-client-vpn-users-with-saml/) | 

#### Service provider information for creating an app<a name="saml-config-service-provider-info"></a>

To create a SAML\-based app using an IdP that's not listed in the preceding table, use the following information to configure the AWS Client VPN service provider information\.
+ Assertion Consumer Service \(ACS\) URL: `http://127.0.0.1:35001`
+ Audience URI: `urn:amazon:webservices:clientvpn`

The following attributes are required\.


| Attribute | Description | 
| --- | --- | 
| NameID | The email address of the user\. | 
| FirstName | The first name of the user\. | 
| LastName | The last name of the user\. | 
| memberOf | The group or groups that the user belongs to\. | 