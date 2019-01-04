# Access to an On\-Premises Network<a name="scenario-onprem"></a>

The configuration for this scenario includes access to an on\-premises network only\. We recommend this configuration if you need to give clients access to the resources inside an on\-premises network only\.

**To implement this configuration**

1. Ensure that you have a VPC with at least one subnet\. Identify the subnet in the VPC that you want to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\. For more information, see [ VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html.html) in the *Amazon VPC User Guide*\.

1. Ensure that the VPC's default security group allows inbound and outbound traffic to and from your clients\. For more information, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

1. Enable communication between the VPC and your own on\-premises network over an AWS Site\-to\-Site VPN connection\. To do this, perform the steps described in [Setting Up an AWS VPN Connection](https://docs.aws.amazon.com/vpc/latest/userguide/SetUpVPNConnections.html) in the *Amazon VPC User Guide*\.

1. Test the AWS Site\-to\-Site VPN you created in the previous step\. To do this, perform the steps described in [Testing the VPN Connection](https://docs.aws.amazon.com/vpc/latest/userguide/HowToTestEndToEnd_Linux.html) in the *Amazon VPC User Guide*\. If the AWS Site\-to\-Site VPN is functioning as expected, continue to the next step\.

1. Create a Client VPN endpoint in the same region as the VPC\. To do this, perform the steps described in [Create a Client VPN Endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet that you identified earlier with the Client VPN endpoint\. To do this, perform the steps described in [Associate a Target Network with a Client VPN Endpoint](cvpn-working-target.md#cvpn-working-target-associate) and select the VPC and the subnet\.

1. Add a route that allows access to the AWS Site\-to\-Site VPN connection\. To do this, perform the steps described in [Create an Endpoint Route](cvpn-working-routes.md#cvpn-working-routes-create); for **Route destination**, enter the Ipv4 CIDR range of the AWS Site\-to\-Site VPN connection, and for **Target VPC Subnet ID**, select the subnet you associated with the Client VPN endpoint\.

1. Add an authorization rule to give clients access to the AWS Site\-to\-Site VPN connection\. To do this, perform the steps described in [Add an Authorization Rule to a Client VPN Endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize); for **Destination network to enable**, enter the AWS Site\-to\-Site VPN connection Ipv4 CIDR range, and for **Grant access to**, select **Allow access to all users**\.