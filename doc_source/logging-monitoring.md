# Logging and monitoring<a name="logging-monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of your Client VPN endpoint\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. AWS provides several tools for monitoring your resources and responding to potential incidents\.

**Amazon CloudWatch**  
*Amazon CloudWatch* monitors your AWS resources and the applications that you run on AWS in real time\. You can collect and track metrics for your Client VPN endpoint\. For more information, see [Monitoring with Amazon CloudWatch](monitoring-cloudwatch.md)\.

**AWS CloudTrail**  
*AWS CloudTrail* captures Amazon EC2 API calls and related events made by or on behalf of your AWS account\. It then delivers the log files to an Amazon S3 bucket that you specify\. For more information, see [Monitoring with AWS CloudTrail](monitoring-cloudtrail.md)\.

**Amazon CloudWatch Logs**  
You can view connection logs to get information about connection events, which are when clients connect, attempt to connect, or disconnect from your Client VPN endpoint\. For more information, see [Connection logging](connection-logging.md)\.