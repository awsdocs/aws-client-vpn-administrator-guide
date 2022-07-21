# Working with connection logs<a name="cvpn-working-with-connection-logs"></a>

You can enable connection logging for a new or existing Client VPN endpoint, and start capturing connection logs\.

Before you begin, you must have a CloudWatch Logs log group in your account\. For more information, see [Working with Log Groups and Log Streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) in the *Amazon CloudWatch Logs User Guide*\. Charges apply for using CloudWatch Logs\. For more information, see [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

When you enable connection logging, you can specify the name of a log stream in the log group\. If you do not specify a log stream, the Client VPN service creates one for you\.

## Enable connection logging for a new Client VPN endpoint<a name="create-connection-log-new"></a>

You can enable connection logging when you create a new Client VPN endpoint by using the console or the command line\.

**To enable connection logging for a new Client VPN endpoint using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**, and then choose **Create Client VPN endpoint\.**

1. Complete the options until you reach the **Connection Logging** section\. For more information about the options, see [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Under **Connection logging**, turn on **Enable log details on client connections**\.

1. For **CloudWatch Logs log group name**, choose the name of the CloudWatch Logs log group\.

1. \(Optional\) For **CloudWatch Logs log stream name**, choose the name of the CloudWatch Logs log stream\.

1. Choose **Create Client VPN endpoint**\.

**To enable connection logging for a new Client VPN endpoint using the AWS CLI**  
Use the [create\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-client-vpn-endpoint.html) command, and specify the `--connection-log-options` parameter\. You can specify the connection logs information in JSON format, as shown in the following example\.

```
{
    "Enabled": true,
    "CloudwatchLogGroup": "ClientVpnConnectionLogs",
    "CloudwatchLogStream": "NewYorkOfficeVPN"
}
```

## Enable connection logging for an existing Client VPN endpoint<a name="create-connection-log-existing"></a>

You can enable connection logging for an existing Client VPN endpoint by using the console or the command line\.

**To enable connection logging for an existing Client VPN endpoint using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint, choose **Actions**, and then choose **Modify Client VPN endpoint**\.

1. Under **Connection logging**, turn on **Enable log details on client connections**\.

1. For **CloudWatch Logs log group name**, choose the name of the CloudWatch Logs log group\.

1. \(Optional\) For **CloudWatch Logs log stream name**, choose the name of the CloudWatch Logs log stream\.

1. Choose **Modify Client VPN endpoint**\.

**To enable connection logging for an existing Client VPN endpoint using the AWS CLI**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command and specify the `--connection-log-options` parameter\. You can specify the connection logs information in JSON format, as shown in the following example\.

```
{
    "Enabled": true,
    "CloudwatchLogGroup": "ClientVpnConnectionLogs",
    "CloudwatchLogStream": "NewYorkOfficeVPN"
}
```

## View connection logs<a name="view-connection-logs"></a>

You can view your connection logs using the CloudWatch Logs console\.

**To view your connection logs using the console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Log groups**, and select the log group that contains your connection logs\. 

1. Select the log stream for your Client VPN endpoint\.
**Note**  
The **Timestamp** column displays the time that the connection log was published to CloudWatch Logs, not the time of the connection\.

For more information about searching log data, see [Search Log Data Using Filter Patterns](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SearchDataFilterPattern.html) in the *Amazon CloudWatch Logs User Guide*\.

## Turn off connection logging<a name="disable-connection-logs"></a>

You can turn off connection logging for a Client VPN endpoint by using the console or the command line\. When you turn off connection logging, existing connection logs in CloudWatch Logs are not deleted\.

**To turn off connection logging using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint, choose **Actions**, and then choose **Modify Client VPN endpoint**\.

1. Under **Connection logging**, turn off **Enable log details on client connections**\.

1. Choose **Modify Client VPN endpoint**\.

**To turn off connection logging using the AWS CLI**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command, and specify the `--connection-log-options` parameter\. Ensure that `Enabled` is set to `false`\.