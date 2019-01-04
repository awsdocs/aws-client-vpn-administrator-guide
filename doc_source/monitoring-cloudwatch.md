# Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

AWS Client VPN publishes the following data points to Amazon CloudWatch for your Client VPN endpoints\. 
+ Egress Bytes
+ Egress Packets
+  Ingress Bytes
+ Ingress Packets
+ Authentication Failures \(count\)
+ Active Connections \(count\)

CloudWatch enables you to retrieve statistics about those data points as an ordered set of time series data, known as metrics\. Think of a metric as a variable to monitor, and the data points as the values of that variable over time\. Each data point has an associated timestamp and an optional unit of measurement\.

You can use metrics to verify that your system is performing as expected\. For example, you can create a CloudWatch alarm to monitor a specified metric and initiate an action \(such as sending a notification to an email address\) if the metric goes outside what you consider an acceptable range\.

For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.