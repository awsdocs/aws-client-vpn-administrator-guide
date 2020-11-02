# Resilience in AWS Client VPN<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. AWS Regions provide multiple physically separated and isolated Availability Zones, which are connected with low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\. 

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

In addition to the AWS global infrastructure, AWS Client VPN offers features to help support your data resiliency and backup needs\.

## Multiple target networks for high availability<a name="high-availability"></a>

You associate a target network with a Client VPN endpoint to enable clients to establish VPN sessions\. Target networks are subnets in your VPC\. Each subnet that you associate with the Client VPN endpoint must belong to a different Availability Zone\. You can associate multiple subnets with a Client VPN endpoint for high availability\. 