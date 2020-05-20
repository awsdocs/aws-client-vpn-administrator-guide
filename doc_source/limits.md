# AWS Client VPN quotas<a name="limits"></a>

Your AWS account has the following quotas related to Client VPN endpoints\. You can request an increase for some of these quotas\. 
+ Number of Client VPN endpoints per Region: 5
+ Number of authorization rules per Client VPN endpoint: 50
+ Number of routes per Client VPN endpoint: 10
+ Number of concurrent client connections per Client VPN endpoint: 2000
+ Number of concurrent operations per Client VPN endpoint: 10

  Operations include:
  + Associating or disassociating subnets
  + Creating or deleting routes
  + Creating or deleting inbound and outbound rules
  + Creating or deleting security groups

For quotas and rules related to configuring users and groups in an identity provider \(IdP\) for SAML\-based federated authentication, see [Requirements and considerations for SAML\-based federated authentication](client-authentication.md#saml-requirements)\.

## General considerations<a name="quotas-general"></a>

Take the following into consideration when you use Client VPN endpoints:
+ The Client VPN endpoint must belong to the same account as the VPC that contains the subnet that you want to associate with the Client VPN endpoint\. 
+ If you use Active Directory to authenticate the user, the Client VPN endpoint must belong to the same account as the AWS Directory Service resource used for Active Directory authentication\.
+ If you use SAML\-based federated authentication to authenticate a user, the Client VPN endpoint must belong to the same account as the IAM SAML identity provider that you create to define the IdP\-to\-AWS trust relationship\. The IAM SAML identity provider can be shared across multiple Client VPN endpoints in the same AWS account\.