# Security best practices for AWS Client VPN<a name="security-best-practices"></a>

AWS Client VPN provides a number of security features to consider as you develop and implement your own security policies\. The following best practices are general guidelines and don’t represent a complete security solution\. Because these best practices might not be appropriate or sufficient for your environment, treat them as helpful considerations rather than prescriptions\.

**Authorization rules**  
Use authorization rules to restrict which users can access your network\. For more information, see [Authorization rules](cvpn-working-rules.md)\.

**Security groups**  
Use security groups to control which resources users can access in your VPC\. For more information, see [Security groups](client-authorization.md#security-groups)\.

**Client certificate revocation lists**  
Use client certificate revocation lists to revoke access to a Client VPN endpoint for specific client certificates\. For example, when a user leaves your organization\. For more information, see [Client certificate revocation lists](cvpn-working-certificates.md)\.

**Monitoring tools**  
Use monitoring tools to keep track of availability and performance of your Client VPN endpoints\. For more information, see [Monitoring Client VPN](monitoring-overview.md)\.

**Identity and access management**  
Manage access to Client VPN resources and APIs by using IAM policies for your IAM users and IAM roles\. For more information, see [Identity and Access Management for AWS Client VPN](security-iam.md)\.