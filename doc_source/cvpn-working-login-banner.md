# Client login banner<a name="cvpn-working-login-banner"></a>

AWS Client VPN provides the option to display a text banner on AWS provided Client VPN desktop applications when a VPN session is established\. You can define the contents of the text banner to meet your regulatory and compliance needs\. A maximum of 1400, UTF\-8 encoded characters can be used\.

**Note**  
When a client login banner has been enabled, it will be displayed on newly created VPN sessions only\. Existing VPN sessions are not interrupted, though the banner will be displayed when an existing session is re\-established\.

See [Release notes for the AWS provided client](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/release-notes.html) in the *AWS Client VPN User Guide* for details on client desktop applications\.

**Topics**
+ [Configure a client login banner during creation of a Client VPN endpoint](#configure-login-banner-endpoint-creation)
+ [Configure a client login banner for an existing Client VPN endpoint](#configure-login-banner-existing-endpoint)
+ [Disable a client login banner for an existing Client VPN endpoint](#disable-login-banner)
+ [Modify existing banner text on a Client VPN endpoint](#modify-banner-text)
+ [View currently configured login banner](#display-login-banner)

## Configure a client login banner during creation of a Client VPN endpoint<a name="configure-login-banner-endpoint-creation"></a>

For detailed steps to enable a client login banner during creation of a Client VPN endpoint, see [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

## Configure a client login banner for an existing Client VPN endpoint<a name="configure-login-banner-existing-endpoint"></a>

Use the following steps to configure a client login banner for an existing Client VPN endpoint\.

**Enable client login banner on a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint that you want to modify, choose **Actions**, and then choose **Modify Client VPN Endpoint**\.

1. Scroll down the page to the **Other Optional Parameters** section\.

1. For **Do you want to enable Client Login Banner?**, choose **Yes**\.

1. For **Client Login Banner Text**, enter the text that will be displayed in a banner on AWS provided clients when a VPN session is established\. Use UTF\-8 encoded characters only, with a maximum of 1400 characters allowed\.

1. Choose **Modify Client VPN Endpoint**\.

**Enable client login banner on a Client VPN endpoint \(AWS CLI\)**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command\.

## Disable a client login banner for an existing Client VPN endpoint<a name="disable-login-banner"></a>

Use the following steps to disable a client login banner for an existing Client VPN endpoint\.

**Disable client login banner on a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint that you want to modify, choose **Actions**, and then choose **Modify Client VPN Endpoint**\.

1. Scroll down the page to the **Other Optional Parameters** section\.

1. For **Do you want to enable Client Login Banner?**, choose **No**\.

1. Choose **Modify Client VPN Endpoint**\.

**Disable client login banner on a Client VPN endpoint \(AWS CLI\)**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command\.

## Modify existing banner text on a Client VPN endpoint<a name="modify-banner-text"></a>

Use the following steps to modify existing text on a client login banner\.

**Modify existing banner text on a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint that you want to modify, choose **Actions**, and then choose **Modify Client VPN Endpoint**\.

1. For **Do you want to enable Client Login Banner?**, verify that **Yes** is selected\.

1. For **Client Login Banner Text**, replace the existing text with new text that you want displayed in a banner on AWS provided clients when a VPN session is established\. Use UTF\-8 encoded characters only, with a maximum of 1400 characters\.

1. Choose **Modify Client VPN Endpoint**\.

**Modify client login banner on a Client VPN endpoint \(AWS CLI\)**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command\.

## View currently configured login banner<a name="display-login-banner"></a>

Use the following steps to view a currently configured login banner\.

**View current login banner for a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint that you want to view\.

1. Verify that the **Summary** tab is selected\.

1. View the currently configured login banner text next to **Client Login Banner text**\. You can also view other details displayed under the **Summary** tab\.

**View currently configured login banner for a Client VPN endpoint \(AWS CLI\)**  
Use the [describe\-client\-vpn\-endpoints](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-endpoints.html) command\.