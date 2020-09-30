# Client\-to\-client access<a name="scenario-client-to-client"></a>

The configuration for this scenario enables clients to access a single VPC, and enables clients to route traffic to each other\. We recommend this configuration if the clients that connect to the same Client VPN endpoint also need to communicate with each other\. Clients can communicate with each other using the unique IP address that's assigned to them from the client CIDR range when they connect to the Client VPN endpoint\.

![\[Client-to-client access\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-scenario-client-to-client.png)

Before you begin, do the following:
+ Create or identify a VPC with at least one subnet\. Identify the subnet in the VPC that you want to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\. For more information, see [ VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.
+ Identify a suitable CIDR range for the client IP addresses that does not overlap with the VPC CIDR\. 
+ Review the rules and limitations for Client VPN endpoints in [Limitations and rules of Client VPN](what-is.md#what-is-limitations)\.

**To implement this configuration**

1. Create a Client VPN endpoint in the same Region as the VPC\. To do this, perform the steps described in [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet that you identified earlier with the Client VPN endpoint\. To do this, perform the steps described in [Associate a target network with a Client VPN endpoint](cvpn-working-target.md#cvpn-working-target-associate) and select the VPC and the subnet\.

1. Add a route to the local network in the route table\. To do this, perform the steps described in [Create an endpoint route](cvpn-working-routes.md#cvpn-working-routes-create)\. For **Route destination**, enter the client CIDR range, and for **Target VPC Subnet ID**, specify `local`\.

1. Add an authorization rule to give clients access to the VPC\. To do this, perform the steps described in [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize)\. For **Destination network to enable **, enter the IPv4 CIDR range of the VPC\.

1. Add an authorization rule to give clients access to the client CIDR range\. To do this, perform the steps described in [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize)\. For **Destination network to enable**, enter the client CIDR range\.