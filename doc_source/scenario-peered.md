# Access to a peered VPC<a name="scenario-peered"></a>

The configuration for this scenario includes a target VPC \(VPC A\) that is peered with an additional VPC \(VPC B\)\. We recommend this configuration if you need to give clients access to the resources inside a target VPC and other VPCs that are peered with it \(such as VPC B\)\.

**Note**  
The procedure for allowing access to a peered VPC outlined below, is only required if the Client VPN endpoint was configured for split\-tunnel mode\. In full\-tunnel mode, access to the peered VPC is allowed by default\.

![\[Client VPN accessing a peer VPC\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-scenario-peer-vpc.png)

Before you begin, do the following:
+ Create or identify a VPC with at least one subnet\. Identify the subnet in the VPC to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\.
+ Identify a suitable CIDR range for the client IP addresses that does not overlap with the VPC CIDR\. 
+ Review the rules and limitations for Client VPN endpoints in [Limitations and rules of Client VPN](what-is.md#what-is-limitations)\.

**To implement this configuration**

1. Establish the VPC peering connection between the VPCs\. Follow the steps at [Creating and accepting a VPC peering connection](https://docs.aws.amazon.com/vpc/latest/peering/create-vpc-peering-connection.html) in the *Amazon VPC Peering Guide*\. Confirm that instances in VPC A can communicate with instances in VPC B using the peering connection\.

1. Create a Client VPN endpoint in the same Region as the target VPC\. In the diagram, this is VPC A\. Perform the steps described in [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet that you identified with the Client VPN endpoint that you created\. To do this, perform the steps described in [Associate a target network with a Client VPN endpoint](cvpn-working-target.md#cvpn-working-target-associate), selecting the VPC and the subnet\. By default, we associate the default security of the VPC with the Client VPN endpoint\. You can associate a different security group using the steps described in [Apply a security group to a target network](cvpn-working-target.md#cvpn-working-target-apply)\.

1. Add an authorization rule to give clients access to the target VPC\. To do this, perform the steps described in [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize)\. For **Destination network to enable **, enter the IPv4 CIDR range of the VPC\.

1. Add a route to direct traffic to the peered VPC\. In the diagram, this is VPC B\. To do this, perform the steps described in [Create an endpoint route](cvpn-working-routes.md#cvpn-working-routes-create)\. For **Route destination**, enter the IPv4 CIDR range of the peered VPC\. For **Target VPC Subnet ID**, select the subnet you associated with the Client VPN endpoint\.

1. Add an authorization rule to give clients access to peered VPC\. To do this, perform the steps described in [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize)\. For **Destination network**, enter the IPv4 CIDR range of the peered VPC\.

1. Add a rule to the security groups for your instances in VPC A and VPC B to allow traffic from the security group that was applied the Client VPN endpoint in step 3\. For more information, see [Security groups](client-authorization.md#security-groups)\.