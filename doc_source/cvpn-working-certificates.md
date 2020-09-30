# Client certificate revocation lists<a name="cvpn-working-certificates"></a>

You can use client certificate revocation lists to revoke access to a Client VPN endpoint for specific client certificates\.

**Note**  
For more information about generating the server and client certificates and keys, see [Mutual authentication](client-authentication.md#mutual)

**Topics**
+ [Generate a client certificate revocation list](#cvpn-working-certificates-generate)
+ [Import a client certificate revocation list](#cvpn-working-certificates-import)
+ [Export a client certificate revocation list](#cvpn-working-certificates-export)

## Generate a client certificate revocation list<a name="cvpn-working-certificates-generate"></a>

------
#### [ Linux/macOS ]

In the following procedure, you generate a client certificate revocation list using the OpenVPN easy\-rsa command line utility\.

**To generate a client certificate revocation list using OpenVPN easy\-rsa**

1. Clone the OpenVPN easy\-rsa repo to your local computer\.

   ```
   $ git clone https://github.com/OpenVPN/easy-rsa.git
   ```

1. Navigate into the `easy-rsa/easyrsa3` folder in your local repo\.

   ```
   $ cd easy-rsa/easyrsa3
   ```

1. Revoke the client certificate and generate the client revocation list\.

   ```
   $ ./easyrsa revoke client_certificate_name
   $ ./easyrsa gen-crl
   ```

   Type `yes` when prompted\.

------
#### [ Windows ]

The following procedure uses the OpenVPN software to generate a client revocation list\. It assumes that you followed the [steps for using the OpenVPN software](client-authentication.md#mutual) to generate the client and server certificates and keys\.

**To generate a client certificate revocation list**

1. Open a command prompt and navigate to the OpenVPN directory\.

   ```
   C:\> cd \Program Files\OpenVPN\easy-rsa
   ```

1. Run the `vars.bat` file\.

   ```
   C:\> vars
   ```

1. Revoke the client certificate and generate the client revocation list\.

   ```
   C:\> revoke-full client_certificate_name
   C:\> more crl.pem
   ```

------

## Import a client certificate revocation list<a name="cvpn-working-certificates-import"></a>

You must have a client certificate revocation list file to import\. For more information about generating a client certificate revocation list, see [Generate a client certificate revocation list](#cvpn-working-certificates-generate)\.

You can import a client certificate revocation list using the console and the AWS CLI\.

**To import a client certificate revocation list \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint for which to import the client certificate revocation list\.

1. Choose **Actions**, and choose **Import Client Certificate CRL**\.

1. For **Certificate Revocation List**, enter the contents of the client certificate revocation list file, and choose **Import CRL**\.

**To import a client certificate revocation list \(AWS CLI\)**  
Use the [import\-client\-vpn\-client\-certificate\-revocation\-list](https://docs.aws.amazon.com/cli/latest/reference/ec2/import-client-vpn-client-certificate-revocation-list.html) command\.

```
$ aws ec2 import-client-vpn-client-certificate-revocation-list --certificate-revocation-list file:path_to_CRL_file --client-vpn-endpoint-id endpoint_id --region region
```

## Export a client certificate revocation list<a name="cvpn-working-certificates-export"></a>

You can export client certificate revocation lists using the console and the AWS CLI\.

**To export a client certificate revocation list \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint for which to export the client certificate revocation list\.

1. Choose **Actions**, choose **Export Client Certificate CRL**, and choose **Yes, Export**\.

**To export a client certificate revocation \(AWS CLI\)**  
Use the [export\-client\-vpn\-client\-certificate\-revocation\-list](https://docs.aws.amazon.com/cli/latest/reference/ec2/export-client-vpn-client-certificate-revocation-list.html) command\.