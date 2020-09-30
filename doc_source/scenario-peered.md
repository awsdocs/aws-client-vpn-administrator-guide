# Access to a peered VPC<a name="scenario-peered"></a>

The configuration for this scenario includes a target VPC \(VPC A\) that is peered with an additional VPC \(VPC B\)\. We recommend this configuration if you need to give clients access to the resources inside a target VPC and other VPCs that are peered with it \(such as VPC B\)\.

![\[Client VPN accessing a peer VPC\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-scenario-peer-vpc.png)

Before you begin, do the following:
+ Create or identify a VPC with at least one subnet\. Identify the subnet in the VPC that you want to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\. For more information, see [ VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.
+ Identify a suitable CIDR range for the client IP addresses that does not overlap with the VPC CIDR\. 
+ Review the rules and limitations for Client VPN endpoints in [Limitations and rules of Client VPN](what-is.md#what-is-limitations)\.

**To implement this configuration**

1. Establish the VPC peering connection between the VPCs\. Follow the steps at [Creating and accepting a VPC peering connection](https://docs.aws.amazon.com/vpc/latest/peering/create-vpc-peering-connection.html) in the *Amazon VPC Peering Guide*\.

1. Test the VPC peering connection\. Confirm that instances in either VPC can communicate with each other as if they are within the same network\. If the peering connection works as expected, continue to the next step\.

1. Create a Client VPN endpoint in the same Region as the target VPC\. In the preceding example, this is VPC A\. Perform the steps described in [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet you identified earlier with the Client VPN endpoint that you created\. To do this, perform the steps described in [Associate a target network with a Client VPN endpoint](cvpn-working-target.md#cvpn-working-target-associate) and select the subnet and the VPC\.

1. Add an authorization rule to give clients access to the target VPC\. To do this, perform the steps described in [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize), and for **Destination network to enable **, enter the IPv4 CIDR range of the VPC\.

1. Add a route to direct traffic to the peered VPC\. In the preceding example, this is VPC B\. To do this, perform the steps described in [Create an endpoint route](cvpn-working-routes.md#cvpn-working-routes-create); for **Route destination**, enter IPv4 CIDR range of the peered VPC, and for **Target VPC Subnet ID**, select the subnet you associated with the Client VPN endpoint\.

1. Add an authorization rule to give clients access to peered VPC\. To do this, perform the steps described in [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize); for **Destination network**, enter IPv4 CIDR range of the peered VPC\.

1. Add a rule to your resources' security groups in VPC A and VPC B to allow traffic from the security group that was applied to the subnet association in step 2\. For more information, see [Security groups](client-authorization.md#security-groups)\.