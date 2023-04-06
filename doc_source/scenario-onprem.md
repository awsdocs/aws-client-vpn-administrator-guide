# Access to an on\-premises network<a name="scenario-onprem"></a>

The configuration for this scenario includes access to an on\-premises network only\. We recommend this configuration if you need to give clients access to the resources inside an on\-premises network only\.

![\[Client VPN accessing an on-premises network\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-scenario-on-premises.png)

Before you begin, do the following:
+ Create or identify a VPC with at least one subnet\. Identify the subnet in the VPC to associate with the Client VPN endpoint and note its IPv4 CIDR ranges\.
+ Identify a suitable CIDR range for the client IP addresses that does not overlap with the VPC CIDR\. 
+ Review the rules and limitations for Client VPN endpoints in [Limitations and rules of Client VPN](what-is.md#what-is-limitations)\.

**To implement this configuration**

1. Enable communication between the VPC and your own on\-premises network over an AWS Site\-to\-Site VPN connection\. To do this, perform the steps described in [Getting started](https://docs.aws.amazon.com/vpn/latest/s2svpn/SetUpVPNConnections.html) in the *AWS Site\-to\-Site VPN User Guide*\. 
**Note**  
Alternatively, you can implement this scenario by using an AWS Direct Connect connection between your VPC and your on\-premises network\. For more information, see the [AWS Direct Connect User Guide](https://docs.aws.amazon.com/directconnect/latest/UserGuide/)\.

1. Test the AWS Site\-to\-Site VPN connection you created in the previous step\. To do this, perform the steps described in [Testing the Site\-to\-Site VPN connection](https://docs.aws.amazon.com/vpn/latest/s2svpn/HowToTestEndToEnd_Linux.html) in the *AWS Site\-to\-Site VPN User Guide*\. If the VPN connection is functioning as expected, continue to the next step\.

1. Create a Client VPN endpoint in the same Region as the VPC\. To do this, perform the steps described in [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Associate the subnet that you identified earlier with the Client VPN endpoint\. To do this, perform the steps described in [Associate a target network with a Client VPN endpoint](cvpn-working-target.md#cvpn-working-target-associate) and select the VPC and the subnet\.

1. Add a route that allows access to the AWS Site\-to\-Site VPN connection\. To do this, perform the steps described in [Create an endpoint route](cvpn-working-routes.md#cvpn-working-routes-create); for **Route destination**, enter the IPv4 CIDR range of the AWS Site\-to\-Site VPN connection, and for **Target VPC Subnet ID**, select the subnet you associated with the Client VPN endpoint\.

1. Add an authorization rule to give clients access to the AWS Site\-to\-Site VPN connection\. To do this, perform the steps described in [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize); for **Destination network**, enter the AWS Site\-to\-Site VPN connection IPv4 CIDR range\.