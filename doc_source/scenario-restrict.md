# Restrict Access to Specific Resources in your VPC<a name="scenario-restrict"></a>

You can grant or deny access to specific resources in your VPC\. 

This configuration expands on the scenario described in [Access to a VPC](scenario-vpc.md)\. This configuration is applied in addition to the authorization rule configured in that scenario\.

## Grant access to Client VPN clients only<a name="scenario-restrict-grant"></a>

This configuration grants only Client VPN clients access to a specific resource in a VPC\.

On the instance on which your resource is running, create a security group rule that allows only traffic from the security group that was applied to the target network association\.

## Deny Client VPN clients access<a name="scenario-restrict-deny"></a>

This configuration prevents Client VPN clients from accessing a specific resource in a VPC\.

On the instance on which your resource is running, the security group must not allow traffic from the security group that was applied to the target network association\.