# Client connections<a name="cvpn-working-connections"></a>

Connections are VPN sessions that have been established by clients\. A connection is established when a client successfully connects to a Client VPN endpoint\.

**Topics**
+ [View client connections](#cvpn-working-connections-view)
+ [Terminate a client connection](#cvpn-working-connections-disassociate)

## View client connections<a name="cvpn-working-connections-view"></a>

You can view client connections using the console and the AWS CLI\.

**To view client connections \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint for which to view client connections\.

1. Choose the **Connections** tab\. The **Connections** tab lists all active and terminated client connections\.

**To view client connections \(AWS CLI\)**  
Use the [describe\-client\-vpn\-connections](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-connections.html) command\.

## Terminate a client connection<a name="cvpn-working-connections-disassociate"></a>

When you terminate a client connection, the VPN session ends\.

You can terminate client connections using the console and the AWS CLI\.

**To terminate a client connection \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to which the client is connected and choose **Connections**\.

1. Select the connection to terminate, choose **Terminate Connection**, and choose **Terminate Connection**\.

**To terminate a client connection \(AWS CLI\)**  
Use the [terminate\-client\-vpn\-connections](https://docs.aws.amazon.com/cli/latest/reference/ec2/terminate-client-vpn-connections.html) command\.