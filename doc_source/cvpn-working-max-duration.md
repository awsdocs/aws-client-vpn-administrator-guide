# VPN session maximum duration<a name="cvpn-working-max-duration"></a>

AWS Client VPN provides several options for the maximum VPN session duration\. You can configure a shorter maximum VPN session duration to meet security and compliance requirements\. By default, the maximum VPN session duration is 24 hours\.

**Note**  
When the maximum VPN session duration value is decreased, active VPN sessions older than the new timeout value will be disconnected\.

See [Release notes for the AWS provided client](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/release-notes.html) in the *AWS Client VPN User Guide* for details on client desktop applications\.

**Topics**
+ [Configure maximum VPN session during creation of a Client VPN endpoint](#configure-max-duration-endpoint-creation)
+ [View current maximum VPN session duration](#display-max-duration)
+ [Modify maximum VPN session duration](#modify-max-timeout)

## Configure maximum VPN session during creation of a Client VPN endpoint<a name="configure-max-duration-endpoint-creation"></a>

For detailed steps for configuring maximum VPN session during creation of a Client VPN endpoint, see [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

## View current maximum VPN session duration<a name="display-max-duration"></a>

Use the following steps to view current maximum VPN session duration\.

**View current maximum VPN session duration for a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint that you want to view\.

1. Verify that the **Details** tab is selected\.

1. View the current maximum VPN session duration next to **Session timeout hours**\. 

**View current maximum VPN session duration for a Client VPN endpoint \(AWS CLI\)**  
Use the [describe\-client\-vpn\-endpoints](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-endpoints.html) command\.

## Modify maximum VPN session duration<a name="modify-max-timeout"></a>

Use the following steps to modify an existing maximum VPN session duration\.

**Modify an existing maximum VPN session duration for a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN endpoints**\.

1. Select the Client VPN endpoint that you want to modify, choose **Actions**, and then choose **Modify Client VPN Endpoint**\.

1. For **Session timeout hours**, choose the desired maximum VPN session duration time in hours\.

1. Choose **Modify Client VPN endpoint**\.

**Modify an existing maximum VPN session duration for a Client VPN endpoint \(AWS CLI\)**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command\.