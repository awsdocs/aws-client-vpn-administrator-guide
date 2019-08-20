# Access to a VPC<a name="scenario-vpc"></a>

The configuration for this scenario includes a single target VPC\. We recommend this configuration if you need to give clients access to the resources inside a single VPC only\.

**To implement this configuration**

1. Ensure that you have a VPC with at least one subnet\. Identify the subnet in the VPC that you want to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\. For more information, see [ VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html.html) in the *Amazon VPC User Guide*\.

1. Ensure that the VPC's default security group allows inbound and outbound traffic to and from the elastic network interface IP address, or the VPC CIDR range \(this option allows you to scale when you add additional elastic network interfaces\)\. \. For more information, see [ Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

   Ensure the subnet you choose does not overlap with the resources that you want to access via the Client VPN endpoint, because the endpoint uses the source NAT \(SNAT\) to connect to resources in the associated VPCs\.

1. Create a Client VPN endpoint in the same region as the VPC\. To do this, perform the steps described in [Create a Client VPN Endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet with the Client VPN endpoint\. To do this, perform the steps described in [Associate a Target Network with a Client VPN Endpoint](cvpn-working-target.md#cvpn-working-target-associate) and select the subnet and the VPC you identified earlier\.

1. Add an authorization rule to give clients access to the VPC\. To do this, perform the steps described in [Add an Authorization Rule to a Client VPN Endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize), and for **Destination network to enable **, enter the IPv4 CIDR range of the VPC\.