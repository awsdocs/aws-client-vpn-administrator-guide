# Export and configure the client configuration file<a name="cvpn-working-endpoint-export"></a>

The Client VPN endpoint configuration file is the file that clients \(users\) use to establish a VPN connection with the Client VPN endpoint\. You must download \(export\) this file and distribute it to all clients who need access to the VPN\. Alternatively, if you've enabled the self\-service portal for your Client VPN endpoint, clients can log into the portal and download the configuration file themselves\. For more information, see [Access the self\-service portal](cvpn-self-service-portal.md)\.

If your Client VPN endpoint uses mutual authentication, you must [add the client certificate and the client private key to the \.ovpn configuration file](#add-config-file-cert-key) that you download\. After you add the information, clients can import the \.ovpn file into the OpenVPN client software\.

**Important**  
If you do not add the client certificate and the client private key information to the file, clients that authenticate using mutual authentication cannot connect to the Client VPN endpoint\.

By default, the “\-\-remote\-random\-hostname” option in the OpenVPN client configuration enables wildcard DNS\. Because wildcard DNS is enabled, the client does not cache the IP address of the endpoint and you will not be able to ping the DNS name of the endpoint\. 

If your Client VPN endpoint uses Active Directory authentication and if you enable multi\-factor authentication \(MFA\) on your directory after you distribute the client configuration file, you must download a new file and redistribute it to your clients\. Clients cannot use the previous configuration file to connect to the Client VPN endpoint\.

## Export the client configuration file<a name="export-client-config-file"></a>

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

## Add the client certificate and key information \(mutual authentication\)<a name="add-config-file-cert-key"></a>

If your Client VPN endpoint uses mutual authentication, you must add the client certificate and the client private key to the \.ovpn configuration file that you download\.

You cannot modify the client certificate when you use mutual authentication\.

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