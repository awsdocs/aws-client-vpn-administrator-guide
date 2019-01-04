# Access to a Peered VPC<a name="scenario-peered"></a>

The configuration for this scenario includes a single VPC and an additional VPC that is peered with the target VPC\. We recommend this configuration if you need to give clients access to the resources inside a target VPC and other VPCs that are peered with it\.

**To implement this configuration**

1. Ensure that you have a VPC with at least one subnet\. Identify the subnet in the VPC that you want to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\. For more information, see [ VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html.html) in the *Amazon VPC User Guide*\.

1. Ensure that the VPC's default security group allows inbound and outbound traffic to and from your clients\. For more information, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

1. Establish the VPC peering connection between the VPCs\. Follow the steps at [Creating and Accepting a VPC Peering Connection](https://docs.aws.amazon.com/vpc/latest/userguide/create-vpc-peering-connection.html) in the *Amazon VPC User Guide*\.

1. Test the VPC peering connection\. Confirm that instances in either VPC can communicate with each other as if they are within the same network\. If the peering connection works as expected, continue to the next step\.

1. Create a Client VPN endpoint in the same region as the VPC identified in **Step 1**\. Perform the steps described in [Create a Client VPN Endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet you identified earlier with the Client VPN endpoint that you created\. To do this, perform the steps described in [Associate a Target Network with a Client VPN Endpoint](cvpn-working-target.md#cvpn-working-target-associate) and select the subnet and the VPC\.

1. Add an authorization rule to give clients access to the VPC\. To do this, perform the steps described in [Add an Authorization Rule to a Client VPN Endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize), and for **Destination network to enable **, enter the IPv4 CIDR range of the VPC\.

1. Add a route to direct traffic to the peered VPC\. To do this, perform the steps described in [Create an Endpoint Route](cvpn-working-routes.md#cvpn-working-routes-create); for **Route destination**, enter IPv4 CIDR range of the peered VPC, and for **Target VPC Subnet ID**, select the subnet you associated with the Client VPN endpoint\.

1. Add an authorization rule to give clients access to peered VPC\. To do this, perform the steps described in [Add an Authorization Rule to a Client VPN Endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize); for **Destination network**, enter IPv4 CIDR range of the peered VPC, and for **Grant access to**, select **Allow access to all users**\.