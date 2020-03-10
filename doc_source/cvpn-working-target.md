# Target Networks<a name="cvpn-working-target"></a>

A target network is a subnet in a VPC\. A Client VPN endpoint must have at least one target network to enable clients to connect to it and establish a VPN connection\. 

**Topics**
+ [Associate a Target Network with a Client VPN Endpoint](#cvpn-working-target-associate)
+ [Apply a Security Group to a Target Network](#cvpn-working-target-apply)
+ [Disassociate a Target Network from a Client VPN Endpoint](#cvpn-working-target-disassociate)
+ [View Target Networks](#cvpn-working-target-view)

## Associate a Target Network with a Client VPN Endpoint<a name="cvpn-working-target-associate"></a>

You can associate one or more target networks \(subnets\) with a Client VPN endpoint\. 

The following rules apply:
+ The subnet must have a CIDR block with at least a /27 bitmask, for example 10\.0\.0\.0/27\. The subnet must also have at least 8 available IP addresses\. 
+ If you associate more that one subnet with a Client VPN endpoint, each subnet must be in a different Availability Zone\. We recommend that you associate at least two subnets to provide Availability Zone redundancy\.
+ If you specified a VPC when you created the Client VPN endpoint, the subnet must be in the same VPC\. If you haven't yet associated a VPC with the Client VPN endpoint, you can choose any subnet in any VPC that exists in the same account as the Client VPN endpoint\. 

  All further subnet associations must be from the same VPC\. To associate a subnet from a different VPC, you must first modify the Client VPN endpoint and change the VPC that's associated with it\. For more information, see [Modify a Client VPN Endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-modify)\.

When you associate a subnet with a Client VPN endpoint, we automatically add the local route of the VPC in which the associated subnet is provisioned to the Client VPN endpoint's route table\.

After you associate the first subnet with the Client VPN endpoint, the Client VPN endpoint's status changes from `pending-associate` to `available` and clients are able to establish a VPN connection\.

**To associate a target network with a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint with which to associate the target network, choose **Associations**, and then choose **Associate**\.

1. For **VPC**, choose the VPC in which the subnet is located\. If you specified a VPC when you created the Client VPN endpoint or if you have previous subnet associations, it must be the same VPC\.

1. For **Subnet to associate**, choose the subnet to associate with the Client VPN endpoint\.

1. Choose **Associate**\.

**To associate a target network with a Client VPN endpoint \(AWS CLI\)**  
Use the [associate\-client\-vpn\-target\-network](https://docs.aws.amazon.com/cli/latest/reference/ec2/associate-client-vpn-target-network.html) command\.

## Apply a Security Group to a Target Network<a name="cvpn-working-target-apply"></a>

When you create a Client VPN endpoint, you can specify the security groups to apply to the target network\. When you associate the first target network with a Client VPN endpoint, we automatically apply the default security group of the VPC in which the associated subnet is located\. For more information, see [Security Groups](authentication-authorization.md#security-groups)\.

You can change the security groups for the Client VPN endpoint\. The security group rules that you require depend on the kind of VPN access you want to configure\. For more information, see [Scenarios and Examples](scenario.md)\.

**To apply a security group to a target network \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to which to apply the security groups\.

1. Choose **Security Groups**, select the current security group, and then choose **Apply Security Groups**\.

1. Select the new security groups in the list and choose **Apply Security Groups**\.

**To apply a security group to a target network \(AWS CLI\)**  
Use the [apply\-security\-groups\-to\-client\-vpn\-target\-network](https://docs.aws.amazon.com/cli/latest/reference/ec2/apply-security-groups-to-client-vpn-target-network.html) command\.

## Disassociate a Target Network from a Client VPN Endpoint<a name="cvpn-working-target-disassociate"></a>

If you disassociate all target networks from a Client VPN endpoint, clients can no longer establish a VPN connection\. When you disassociate a subnet, we remove the route that was automatically created when the association was made\.

**To disassociate a target network from a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint with which the target network is associated and choose **Associations**\.

1. Select the target network to disassociate, choose **Disassociate**, and then choose **Yes, Disassociate**\.

**To disassociate a target network from a Client VPN endpoint \(AWS CLI\)**  
Use the [disassociate\-client\-vpn\-target\-network](https://docs.aws.amazon.com/cli/latest/reference/ec2/disassociate-client-vpn-target-network.html) command\.

## View Target Networks<a name="cvpn-working-target-view"></a>

You can view the targets associated with a Client VPN endpoint using the console or the AWS CLI\.

**To view target networks \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint and choose **Associations**\.

**To view target networks using the AWS CLI**  
Use the [describe\-client\-vpn\-target\-networks](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-target-networks.html) command\.