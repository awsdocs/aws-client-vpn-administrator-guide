# Authorization<a name="client-authorization"></a>

Client VPN supports two types of authorization: security groups and network\-based authorization \(using authorization rules\)\.

## Security groups<a name="security-groups"></a>

Client VPN automatically integrates with VPC security groups\. The security groups are associated with the Client VPN network interfaces\. When you create a Client VPN endpoint, you can specify the security groups from a specific VPC to apply to the Client VPN endpoint\. When you associate a subnet with a Client VPN endpoint, we automatically apply the VPC's default security group\. You can change the security groups after you create the Client VPN endpoint\. 

You can enable Client VPN users to access your applications in a VPC by adding a rule to your applications' security groups to allow traffic from the security group that was applied to the association\. Conversely, you can restrict access for Client VPN users, by not specifying the security group that was applied to the association\. For more information, see [Apply a security group to a target network](cvpn-working-target.md#cvpn-working-target-apply)\. The security group rules that you require might also depend on the kind of VPN access you want to configure\. For more information, see [Scenarios and examples](scenario.md)\.

For more information about security groups, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.

## Network\-based authorization<a name="auth-rules"></a>

Network\-based authorization is implemented using authorization rules\. For each network that you want to enable access, you must configure authorization rules that limit the users who have access\. For a specified network, you configure the Active Directory group or the SAML\-based IdP group that is allowed access\. Only users who belong to the specified group can access the specified network\. If you are not using Active Directory or SAML\-based federated authentication, or you want to open access to all users, you can specify a rule that grants access to all clients\. For more information, see [Authorization rules](cvpn-working-rules.md)\.