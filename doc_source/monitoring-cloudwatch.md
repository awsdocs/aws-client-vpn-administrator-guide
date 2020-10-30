# Monitoring with Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

AWS Client VPN publishes the following metrics to Amazon CloudWatch for your Client VPN endpoints\. Metrics are published to Amazon CloudWatch every five minutes\.


| Metric | Description | 
| --- | --- | 
| ActiveConnectionsCount | The number of active connections to the Client VPN endpoint\.Units: Count | 
| AuthenticationFailures | The number of authentication failures for the Client VPN endpoint\.Units: Count | 
| CrlDaysToExpiry |  The number of days until the Certificate Revocation List \(CRL\) which is configured on the Client VPN endpoint expires\. Units: Days  | 
| EgressBytes | The number of bytes sent from the Client VPN endpoint\.Units: Bytes | 
| EgressPackets | The number of packets sent from the Client VPN endpoint\.Units: Count | 
| IngressBytes | The number of bytes received by the Client VPN endpoint\.Units: Bytes | 
| IngressPackets | The number of packets received by the Client VPN endpoint\.Units: Count | 
| SelfServicePortalClientConfigurationDownloads |  The number of downloads of the Client VPN endpoint configuration file from the self\-service portal\. Unit: Count  | 

You can filter the metrics for your Client VPN endpoint by endpoint\.

CloudWatch enables you to retrieve statistics about those data points as an ordered set of time series data, known as metrics\. Think of a metric as a variable to monitor, and the data points as the values of that variable over time\. Each data point has an associated timestamp and an optional unit of measurement\.

You can use metrics to verify that your system is performing as expected\. For example, you can create a CloudWatch alarm to monitor a specified metric and initiate an action \(such as sending a notification to an email address\) if the metric goes outside what you consider an acceptable range\.

For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.