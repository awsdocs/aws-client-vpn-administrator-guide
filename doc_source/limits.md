# AWS Client VPN quotas<a name="limits"></a>

Your AWS account has the following quotas related to Client VPN endpoints\.

## Client VPN quotas<a name="quotas-endpoints"></a>

The following quotas apply to Client VPN\. Unless indicated otherwise, you can request an increase for these quotas\. 
+ Number of Client VPN endpoints per Region: 5
+ Number of authorization rules per Client VPN endpoint: 50
+ Number of routes per Client VPN endpoint: 10
+ Number of concurrent client connections per Client VPN endpoint: 20,000
+ Number of entries in a client certificate revocation list: 20,000

  This quota cannot be increased\.
+ Number of concurrent operations per Client VPN endpoint: 10

  Operations include:
  + Associating or disassociating subnets
  + Creating or deleting routes
  + Creating or deleting inbound and outbound rules
  + Creating or deleting security groups

  This quota cannot be increased\.

## Users and groups quotas<a name="quotas-users-groups"></a>

When you configure users and groups for Active Directory or a SAML\-based IdP, the following quotas apply:
+ Users can belong to a maximum of 200 groups\. We ignore any groups after the 200th group\.
+ The maximum length for the group ID is 255 characters\.
+ The maximum length for the name ID is 255 characters\. We truncate characters after the 255th character\.

## General considerations<a name="quotas-general"></a>

Take the following into consideration when you use Client VPN endpoints:
+ If you use Active Directory to authenticate the user, the Client VPN endpoint must belong to the same account as the AWS Directory Service resource used for Active Directory authentication\.
+ If you use SAML\-based federated authentication to authenticate a user, the Client VPN endpoint must belong to the same account as the IAM SAML identity provider that you create to define the IdP\-to\-AWS trust relationship\. The IAM SAML identity provider can be shared across multiple Client VPN endpoints in the same AWS account\.