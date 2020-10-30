# Monitoring Client VPN<a name="monitoring-overview"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of AWS Client VPN and your other AWS solutions\. You can use the following features to monitor your Client VPN endpoints, analyze traffic patterns, and troubleshoot issues with your Client VPN endpoints\.

**Amazon CloudWatch**  
Monitors your AWS resources and the applications you run on AWS in real time\. You can collect and track metrics, create customized dashboards, and set alarms that notify you or take actions when a specified metric reaches a threshold that you specify\. For example, you can have CloudWatch track CPU usage or other metrics of your Amazon EC2 instances and automatically launch new instances when needed\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**AWS CloudTrail**  
Captures API calls and related events made by or on behalf of your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

**Amazon CloudWatch Logs**  
Enables you to monitor connection attempts made to your AWS Client VPN endpoint\. You can view the connection attempts and connection resets for the Client VPN connections\. For the connection attempts, you can see both the successful and failed connection attempts\. You can specify the CloudWatch Logs log stream to log the connection details\. For more information, see [Connection logging](connection-logging.md) and the [Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\.