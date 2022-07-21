# Access the self\-service portal<a name="cvpn-self-service-portal"></a>

If you enabled the self\-service portal for your Client VPN endpoint, you can provide your clients with a self\-service portal URL\. Clients can access the portal in a web browser, and use their user\-based credentials to log in\. In the portal, clients can download the Client VPN endpoint configuration file and they can download the latest version of the AWS provided client\.

The following rules apply:
+ The self\-service portal is not available for clients that authenticate using mutual authentication\.
+ The configuration file that's available in the self\-service portal is the same configuration file that you export using the Amazon VPC console or AWS CLI\. If you need to customize the configuration file before distributing it to clients, you must distribute the customized file to clients yourself\.
+ You must enable the self\-service portal option for your Client VPN endpoint, or clients cannot access the portal\. If this option is not enabled, you can modify your Client VPN endpoint to enable it\.

After you have enabled the self\-service portal option, provide your clients with one of the following URLs:
+ `https://self-service.clientvpn.amazonaws.com/`

  If clients access the portal using this URL, they must enter the ID of the Client VPN endpoint before they can log in\.
+ `https://self-service.clientvpn.amazonaws.com/endpoints/<endpoint-id>`

  Replace *<endpoint\-id>* in the preceding URL with the ID of your Client VPN endpoint, for example, `cvpn-endpoint-0123456abcd123456`\.

You can also view the URL for the self\-service portal in the output of the [describe\-client\-vpn\-endpoints](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-endpoints.html) AWS CLI command\. Alternatively, the URL is available in the **Details** tab on the **Client VPN Endpoints** page in the Amazon VPC console\.

For more information about configuring the self\-service portal for use with federated authentication, see [Support for the self\-service portal](client-authentication.md#saml-self-service-support)\.