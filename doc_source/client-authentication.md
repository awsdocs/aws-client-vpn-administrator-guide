# Client authentication<a name="client-authentication"></a>

Client authentication is implemented at the first point of entry into the AWS Cloud\. It is used to determine whether clients are allowed to connect to the Client VPN endpoint\. If authentication succeeds, clients connect to the Client VPN endpoint and establish a VPN session\. If authentication fails, the connection is denied and the client is prevented from establishing a VPN session\.

Client VPN offers the following types of client authentication: 
+ [Active Directory authentication](#ad) \(user\-based\)
+ [Mutual authentication](#mutual) \(certificate\-based\)
+ [Single sign\-on \(SAML\-based federated authentication\)](#federated-authentication) \(user\-based\)

You can use one of methods listed above alone, or a combination of mutual authentication with a user\-based method such as the following:
+ Mutual authentication and federated authentication
+ Mutual authentication and Active Directory authentication

**Important**  
To create a Client VPN endpoint, you must provision a server certificate in AWS Certificate Manager, regardless of the type of authentication you use\. For more information about creating and provisioning a server certificate, see the steps in [Mutual authentication](#mutual)\.

## Active Directory authentication<a name="ad"></a>

Client VPN provides Active Directory support by integrating with AWS Directory Service\. With Active Directory authentication, clients are authenticated against existing Active Directory groups\. Using AWS Directory Service, Client VPN can connect to existing Active Directories provisioned in AWS or in your on\-premises network\. This allows you to use your existing client authentication infrastructure\. If you are using an on\-premises Active Directory and you do not have an existing AWS Managed Microsoft AD, you must configure an Active Directory Connector \(AD Connector\)\. You can use one Active Directory server to authenticate the users\. For more information about Active Directory integration, see the [AWS Directory Service Administration Guide](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

Client VPN supports multi\-factor authentication \(MFA\) when it's enabled for AWS Managed Microsoft AD or AD Connector\. If MFA is enabled, clients must enter a user name, password, and MFA code when they connect to a Client VPN endpoint\. For more information about enabling MFA, see [Enable Multi\-Factor Authentication for AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_mfa.html) and [Enable Multi\-Factor Authentication for AD Connector](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ad_connector_mfa.html) in the *AWS Directory Service Administration Guide*\. 

For quotas and rules for configuring users and groups in Active Directory, see [Users and groups quotas](limits.md#quotas-users-groups)\.

## Mutual authentication<a name="mutual"></a>

With mutual authentication, Client VPN uses certificates to perform authentication between the client and the server\. Certificates are a digital form of identification issued by a certificate authority \(CA\)\. The server uses client certificates to authenticate clients when they attempt to connect to the Client VPN endpoint\. You must create a server certificate and key, and at least one client certificate and key\.

You must upload the server certificate to AWS Certificate Manager \(ACM\) and specify it when you create a Client VPN endpoint\. When you upload the server certificate to ACM, you also specify the certificate authority \(CA\)\. You only need to upload the client certificate to ACM when the CA of the client certificate is different from the CA of the server certificate\. For more information about ACM, see the [AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)\. 

You can create a separate client certificate and key for each client that will connect to the Client VPN endpoint\. This enables you to revoke a specific client certificate if a user leaves your organization\. In this case, when you create the Client VPN endpoint, you can specify the server certificate ARN for the client certificate, provided that the client certificate has been issued by the same CA as the server certificate\.

**Note**  
A Client VPN endpoint supports 1024\-bit and 2048\-bit RSA key sizes only\. Also, the client certificate must have the CN attribute in the Subject field\.

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

1. To build a new certificate authority \(CA\), run this command and follow the prompts\.

   ```
   $ ./easyrsa build-ca nopass
   ```

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

1. Upload the server certificate and key and the client certificate and key to ACM\. Be sure to upload them in the same Region in which you intend to create the Client VPN endpoint\. The following commands use the AWS CLI to upload the certificates\. To upload the certificates using the ACM console instead, see [Import a certificate](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-api-cli.html) in the *AWS Certificate Manager User Guide*\.

   ```
   $ aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt
   ```

   ```
   $ aws acm import-certificate --certificate fileb://client1.domain.tld.crt --private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt
   ```

   You do not necessarily need to upload the client certificate to ACM\. If the server and client certificates have been issued by the same Certificate Authority \(CA\), you can use the server certificate ARN for both server and client when you create the Client VPN endpoint\. In the steps above, the same CA has been used to create both certificates\. However, the steps to upload the client certificate are included for completeness\.

------
#### [ Windows ]

The following procedure installs Easy\-RSA 3\.x software and uses it to generate server and client certificates and keys\.

**To generate server and client certificates and keys and upload them to ACM**

1. Open the [EasyRSA releases](https://github.com/OpenVPN/easy-rsa/releases) page and download the ZIP file for your version of Windows and extract it\.

1. Open a command prompt and navigate to the location that the `EasyRSA-3.x` folder was extracted to\.

1. Run the following command to open the EasyRSA 3 shell\.

   ```
   C:\Program Files\EasyRSA-3.x> .\EasyRSA-Start.bat
   ```

1. Initialize a new PKI environment\.

   ```
   # ./easyrsa init-pki
   ```

1. To build a new certificate authority \(CA\), run this command and follow the prompts\.

   ```
   # ./easyrsa build-ca nopass
   ```

1. Generate the server certificate and key\.

   ```
   # ./easyrsa build-server-full server nopass
   ```

1. Generate the client certificate and key\.

   ```
   # ./easyrsa build-client-full client1.domain.tld nopass
   ```

   You can optionally repeat this step for each client \(end user\) that requires a client certificate and key\.

1. Exit the EasyRSA 3 shell\.

   ```
   # exit
   ```

1. Copy the server certificate and key and the client certificate and key to a custom folder and then navigate into the custom folder\.

   Before you copy the certificates and keys, create the custom folder by using the `mkdir` command\. The following example creates a custom folder in your C:\\ drive\.

   ```
   C:\Program Files\EasyRSA-3.x> mkdir C:\custom_folder
   C:\Program Files\EasyRSA-3.x> copy pki\ca.crt C:\custom_folder
   C:\Program Files\EasyRSA-3.x> copy pki\issued\server.crt C:\custom_folder
   C:\Program Files\EasyRSA-3.x> copy pki\private\server.key C:\custom_folder
   C:\Program Files\EasyRSA-3.x> copy pki\issued\client1.domain.tld.crt C:\custom_folder
   C:\Program Files\EasyRSA-3.x> copy pki\private\client1.domain.tld.key C:\custom_folder
   C:\Program Files\EasyRSA-3.x> cd C:\custom_folder
   ```

1. Upload the server certificate and key and the client certificate and key to ACM\. Be sure to upload them in the same Region in which you intend to create the Client VPN endpoint\. The following commands use the AWS CLI to upload the certificates\. To upload the certificates using the ACM console instead, see [Import a certificate](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-api-cli.html) in the *AWS Certificate Manager User Guide*\.

   ```
   aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt
   ```

   ```
   aws acm import-certificate --certificate fileb://client1.domain.tld.crt --private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt
   ```

   You do not necessarily need to upload the client certificate to ACM\. If the server and client certificates have been issued by the same Certificate Authority \(CA\), you can use the server certificate ARN for both server and client when you create the Client VPN endpoint\. In the steps above, the same CA has been used to create both certificates\. However, the steps to upload the client certificate are included for completeness\.

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

1. Export the [client configuration file](cvpn-working-endpoint-export.md) and distribute it to your users\. Instruct your users to download the latest version of the [AWS provided client](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/connect-aws-client-vpn-connect.html), and to use it to load the configuration file and connect to the Client VPN endpoint\. Alternatively, if you enabled the self\-service portal for your Client VPN endpoint, instruct your users to go to the self\-service portal to get the configuration file and AWS provided client\. For more information, see [Access the self\-service portal](cvpn-self-service-portal.md)\.

### Authentication workflow<a name="federated-authentication-workflow"></a>

The following diagram provides an overview of the authentication workflow for a Client VPN endpoint that uses SAML\-based federated authentication\. When you create and configure the Client VPN endpoint, you specify the IAM SAML identity provider\.

![\[Authentication workflow\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/federated-auth-workflow.png)

1. The user opens the AWS provided client on their device and initiates a connection to the Client VPN endpoint\.

1. The Client VPN endpoint sends an IdP URL and authentication request back to the client, based on the information that was provided in the IAM SAML identity provider\.

1. The AWS provided client opens a new browser window on the user's device\. The browser makes a request to the IdP and displays a login page\.

1. The user enters their credentials on the login page, and the IdP sends a signed SAML assertion back to the client\.

1. The AWS provided client sends the SAML assertion to the Client VPN endpoint\.

1. The Client VPN endpoint validates the assertion and either allows or denies access to the user\.

### Requirements and considerations for SAML\-based federated authentication<a name="saml-requirements"></a>

The following are the requirements and considerations for SAML\-based federated authentication\.
+ For quotas and rules for configuring users and groups in a SAML\-based IdP, see [Users and groups quotas](limits.md#quotas-users-groups)\.
+ The SAML assertion and SAML documents must be signed\.
+ AWS Client VPN only supports "AudienceRestriction" and "NotBefore and NotOnOrAfter" conditions in SAML assertions\.
+ The maximum supported size for SAML responses is 128 KB\.
+ AWS Client VPN does not provide signed authentication requests\.
+ SAML single logout is not supported\. Users can log out by disconnecting from the AWS provided client, or you can [terminate the connections](cvpn-working-connections.md#cvpn-working-connections-disassociate)\.
+ A Client VPN endpoint supports a single IdP only\.
+ Multi\-factor authentication \(MFA\) is supported when it's enabled in your IdP\.
+ Users must use the AWS provided client to connect to the Client VPN endpoint\. They must use version 1\.2\.0 or later\. For more information, see [Connect using the AWS provided client](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/connect-aws-client-vpn-connect.html)\.
+ The following browsers are supported for IdP authentication: Apple Safari, Google Chrome, Microsoft Edge, and Mozilla Firefox\.
+ The AWS provided client reserves TCP port 35001 on users' devices for the SAML response\.
+ If the metadata document for the IAM SAML identity provider is updated with an incorrect or malicious URL, this can cause authentication issues for users, or result in phishing attacks\. Therefore, we recommend that you use AWS CloudTrail to monitor updates that are made to the IAM SAML identity provider\. For more information, see [Logging IAM and AWS STS calls with AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html) in the *IAM User Guide*\.
+ AWS Client VPN sends an AuthN request to the IdP via an HTTP Redirect binding\. Therefore, the IdP should support HTTP Redirect binding and it should be present in the IdP's metadata document\.
+ For the SAML assertion, you must use an email address format for the `NameID` attribute\.

### SAML\-based IdP configuration resources<a name="saml-config-resources"></a>

The following table lists the SAML\-based IdPs that we have tested for use with AWS Client VPN, and resources that can help you configure the IdP\.


| IdP | Resource | 
| --- | --- | 
| Okta | [Authenticate AWS Client VPN users with SAML](https://aws.amazon.com/blogs/networking-and-content-delivery/authenticate-aws-client-vpn-users-with-saml/) | 
| Microsoft Azure Active Directory | For more information, see [Tutorial: Azure Active Directory single sign\-on \(SSO\) integration with AWS ClientVPN](https://docs.microsoft.com/en-gb/azure/active-directory/saas-apps/aws-clientvpn-tutorial) on the Microsoft documentation website\. | 

#### Service provider information for creating an app<a name="saml-config-service-provider-info"></a>

To create a SAML\-based app using an IdP that's not listed in the preceding table, use the following information to configure the AWS Client VPN service provider information\.
+ Assertion Consumer Service \(ACS\) URL: `http://127.0.0.1:35001`
+ Audience URI: `urn:amazon:webservices:clientvpn`

The following attribute is required\.


| Attribute | Description | 
| --- | --- | 
| memberOf | The group or groups that the user belongs to\. | 

Attributes are case\-sensitive, and must be configured exactly as specified\.

#### Support for the self\-service portal<a name="saml-self-service-support"></a>

If you enable the self\-service portal for your Client VPN endpoint, users log into the portal using their SAML\-based IdP credentials\.

If your IdP supports multiple Assertion Consumer Service \(ACS\) URLs, add the following ACS URL to your app\.

```
https://self-service.clientvpn.amazonaws.com/api/auth/sso/saml
```

If you are using the Client VPN endpoint in a GovCloud region, use the following ACS URL instead\. If you use the same IDP app to authenticate for both standard and GovCloud regions, you can add both URLs\.

```
https://gov.self-service.clientvpn.amazonaws.com/api/auth/sso/saml
```

If your IdP does not support multiple ACS URLs, do the following: 

1. Create an additional SAML\-based app in your IdP and specify the following ACS URL\.

   ```
   https://self-service.clientvpn.amazonaws.com/api/auth/sso/saml
   ```

1. Generate and download a federation metadata document\.

1. Create an IAM SAML identity provider in the same AWS account as the Client VPN endpoint\. For more information, see [Creating IAM SAML Identity Providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html) in the *IAM User Guide*\. 
**Note**  
You create this IAM SAML identity provider in addition to the one you [create for the main app](#federated-authentication)\.

1. [Create the Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create), and specify both of the IAM SAML identity providers that you created\.