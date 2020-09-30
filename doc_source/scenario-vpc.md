# Access to a VPC<a name="scenario-vpc"></a>

The configuration for this scenario includes a single target VPC\. We recommend this configuration if you need to give clients access to the resources inside a single VPC only\.

![\[Client VPN accessing a VPC\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-scenario-vpc.png)

Before you begin, do the following:
+ Create or identify a VPC with at least one subnet\. Identify the subnet in the VPC that you want to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\. For more information, see [ VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.
+ Identify a suitable CIDR range for the client IP addresses that does not overlap with the VPC CIDR\. 
+ Review the rules and limitations for Client VPN endpoints in [Limitations and rules of Client VPN](what-is.md#what-is-limitations)\.

**To implement this configuration**

1. Create a Client VPN endpoint in the same Region as the VPC\. To do this, perform the steps described in [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet with the Client VPN endpoint\. To do this, perform the steps described in [Associate a target network with a Client VPN endpoint](cvpn-working-target.md#cvpn-working-target-associate) and select the subnet and the VPC you identified earlier\.

1. Add an authorization rule to give clients access to the VPC\. To do this, perform the steps described in [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize), and for **Destination network**, enter the IPv4 CIDR range of the VPC\.

1. Add a rule to your resources' security groups to allow traffic from the security group that was applied to the subnet association in step 2\. For more information, see [Security groups](client-authorization.md#security-groups)\.