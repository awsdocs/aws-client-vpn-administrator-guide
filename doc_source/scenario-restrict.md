# Restrict access to specific resources in your VPC<a name="scenario-restrict"></a>

You can grant or deny access to specific resources in your VPC by adding or removing security group rules that reference the security group that was applied to the target network association \(the Client VPN security group\)\.

This configuration expands on the scenario described in [Access to a VPC](scenario-vpc.md)\. This configuration is applied in addition to the authorization rule configured in that scenario\.

Before you begin, check if the Client VPN security group is associated with other resources in your VPC\. If you add or remove rules that reference the Client VPN security group, you might grant or deny access for the other associated resources too\. To prevent this, create a security group specifically for use with your Client VPN endpoint\.

## Grant access to Client VPN clients only<a name="scenario-restrict-grant"></a>

This configuration grants only Client VPN clients access to a specific resource in a VPC\.

On the security group that's associated with the instance on which your resource is running, create a security group rule that allows only traffic from the Client VPN security group\.

**To create a security group rule**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Security Groups**\.

1. Choose the security group that's associated with the instance on which your resource is running\.

1. Choose **Actions**, **Edit inbound rules**\.

1. Choose **Add rule**, and then do the following:
   + For **Type**, choose **All traffic**, or a specific type of traffic that you want to allow\.
   + For **Source**, choose **Custom**, and then enter or choose the ID of the Client VPN security group\.

1. Choose **Save rules**

## Deny Client VPN clients access<a name="scenario-restrict-deny"></a>

This configuration prevents Client VPN clients from accessing a specific resource in a VPC\.

On the instance on which your resource is running, the security group must not allow traffic from the Client VPN security group\.

**To check your security group rules**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Security Groups**\.

1. Choose **Inbound Rules**\.

1. Review the list of rules\. If there is a rule where **Source** is the Client VPN security group, choose **Edit Rules**, and choose **Delete** \(the x icon\) for the rule\. Choose **Save rules**\.