# Restrict access to your network<a name="scenario-restrict"></a>

You can configure your Client VPN endpoint to restrict access to specific resources in your VPC\. For user\-based authentication, you can also restrict access to parts of your network, based on the user group that accesses the Client VPN endpoint\.

## Restrict access using security groups<a name="scenario-restrict-security-groups"></a>

You can grant or deny access to specific resources in your VPC by adding or removing security group rules that reference the security group that was applied to the target network association \(the Client VPN security group\)\. This configuration expands on the scenario described in [Access to a VPC](scenario-vpc.md)\. This configuration is applied in addition to the authorization rule configured in that scenario\.

To grant access to a specific resource, identify the security group that's associated with the instance on which your resource is running\. Then, create a rule that allows traffic from the Client VPN security group\. 

In the following example, `sg-xyz` is the Client VPN security group, security group `sg-aaa` is associated with instance A, and security group `sg-bbb` is associated with instance B\. You add a rule to `sg-aaa` that allows access from `sg-xyz`, therefore, clients can access your resources in instance A\. Security group `sg-bbb` does not have a rule that allows access from `sg-xyz` or the Client VPN network interface\. Clients cannot access the resources in instance B\.

![\[Restricting access to resources in a VPC\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-scenario-security-groups.png)

Before you begin, check if the Client VPN security group is associated with other resources in your VPC\. If you add or remove rules that reference the Client VPN security group, you might grant or deny access for the other associated resources too\. To prevent this, use a security group that is specifically created for use with your Client VPN endpoint\.

**To create a security group rule**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Security Groups**\.

1. Choose the security group that's associated with the instance on which your resource is running\.

1. Choose **Actions**, **Edit inbound rules**\.

1. Choose **Add rule**, and then do the following:
   + For **Type**, choose **All traffic**, or a specific type of traffic that you want to allow\.
   + For **Source**, choose **Custom**, and then enter or choose the ID of the Client VPN security group\.

1. Choose **Save rules**

To remove access to a specific resource, check the security group that's associated with the instance on which your resource is running\. If there is a rule that allows traffic from the Client VPN security group, delete it\.

**To check your security group rules**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Security Groups**\.

1. Choose **Inbound Rules**\.

1. Review the list of rules\. If there is a rule where **Source** is the Client VPN security group, choose **Edit Rules**, and choose **Delete** \(the x icon\) for the rule\. Choose **Save rules**\.

## Restrict access based on user groups<a name="scenario-restrict-groups"></a>

If your Client VPN endpoint is configured for user\-based authentication, you can grant specific groups of users access to specific parts of your network\. To do this, complete the following steps:

1. Configure users and groups in AWS Directory Service or your IdP\. For more information, see the following topics:
   + [Active Directory authentication](client-authentication.md#ad)
   + [Requirements and considerations for SAML\-based federated authentication](client-authentication.md#saml-requirements)

1. Create an authorization rule for your Client VPN endpoint that allows a specified group access to all or part of your network\. For more information, see [Authorization rules](cvpn-working-rules.md)\.

If your Client VPN endpoint is configured for mutual authentication, you cannot configure user groups\. When you create an authorization rule, you must grant access to all users\. To enable specific groups of users access to specific parts of your network, you can create multiple Client VPN endpoints\. For example, for each group of users that accesses your network, do the following:

1. Create a set of server and client certificates and keys for that group of users\. For more information, see [Mutual authentication](client-authentication.md#mutual)\.

1. Create a Client VPN endpoint\. For more information, see [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

1. Create an authorization rule that grants access to all or part of your network\. For example, for a Client VPN endpoint that is used by administrators, you might create an authorization rule that grants access to the entire network\. For more information, see [Add an authorization rule to a Client VPN endpoint](cvpn-working-rules.md#cvpn-working-rule-authorize)\.