# Access to the Internet<a name="scenario-internet"></a>

The configuration for this scenario includes a single target VPC and access to the internet\. We recommend this configuration if you need to give clients access to the resources inside a single target VPC and allow access to the internet\.

If you completed the [Getting Started with Client VPN](cvpn-getting-started.md) tutorial, then you've already implemented this scenario\.

**To implement this configuration**

1. Ensure that you have a VPC with at least one subnet\. Identify the subnet in the VPC that you want to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\. For more information, see [ VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html.html) in the *Amazon VPC User Guide*\.

1. Ensure that the VPC's default security group allows inbound and outbound traffic to and from your clients\. For more information, see [ Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

1. Ensure that the VPC's default security group allows inbound and outbound traffic to and from the internet\. To do this, add inbound and outbound rules that allow traffic to and from 0\.0\.0\.0/0\.

1. Create an internet gateway and attach it to your VPC\. For more information, see [Creating and Attaching an Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway) in the *Amazon VPC User Guide*\.

1. Make your subnet public by adding a route to the internet gateway to its route table\. In the VPC console, choose **Subnets**, select the subnet you intend to associate with the Client VPN endpoint, choose **Route Table**, and then choose the route table ID\. Choose **Actions**, choose **Edit routes**, and choose **Add route**\. For **Destination**, enter `0.0.0.0/0`, and for **Target**, choose the internet gateway from the previous step\.

1. Create a Client VPN endpoint in the same region as the VPC\. To do this, perform the steps described in [Create a Client VPN Endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet that you identified earlier with the Client VPN endpoint\. To do this, perform the steps described in [Associate a Target Network with a Client VPN Endpoint](cvpn-working-target.md#cvpn-working-target-associate) and select the VPC and the subnet\.

1. Add an authorization rule to give clients access to the VPC\. To do this, perform the steps described in [Add an Authorization Rule to a Client VPN Endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize); and for **Destination network to enable **, enter the IPv4 CIDR range of the VPC\.

1. Add a route that enables traffic to the Internet\. To do this, perform the steps described in [Create an Endpoint Route](cvpn-working-routes.md#cvpn-working-routes-create); for **Route destination**, enter `0.0.0.0/0`, and for **Target VPC Subnet ID**, select the subnet you associated with the Client VPN endpoint\.

1. Add an authorization rule to give clients access Internet\. To do this, perform the steps described in [Add an Authorization Rule to a Client VPN Endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize); for **Destination network to enable**, enter `0.0.0.0/0`, and for **Grant access to**, select **Allow access to all users**\.