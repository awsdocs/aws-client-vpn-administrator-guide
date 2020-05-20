# Authorization rules<a name="cvpn-working-rules"></a>

Authorization rules act as ﬁrewall rules that grant access to networks\. You should have an authorization rule for each network for which you want to grant access\.

**Topics**
+ [Add an authorization rule to a Client VPN endpoint](#cvpn-working-rule-authorize)
+ [Remove an authorization rule from a Client VPN endpoint](#cvpn-working-rule-revoke)
+ [View authorization rules](#cvpn-working-rule-view)

## Add an authorization rule to a Client VPN endpoint<a name="cvpn-working-rule-authorize"></a>

By adding authorization rules, you grant the specific clients access to the specified network\. 

You can add authorization rules to a Client VPN endpoint using the console and the AWS CLI\.

**To add an authorization rule to a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to which to add the authorization rule, choose **Authorization**, and choose **Authorize ingress**\.

1. For **Destination network**, enter the IP address, in CIDR notation, of the network that you want users to access \(for example, the CIDR block of your VPC\)\.

1. Specify which clients are allowed to access the specified network\. For **For grant access to**, do one of the following:
   + To grant access to all clients, choose **Allow access to all users**\.
   + To restrict access to specific clients, choose **Allow access to users in a specific access group**, and then for **Access group ID**, enter the ID for the group to grant access to\. For example, the security identifier \(SID\) of an Active Directory group, or the ID/name of a group defined in a SAML\-based identity provider \(IdP\)\.
**Note**  
\(Active Directory\) You can use the Microsoft Powershell [Get\-ADGroup](https://docs.microsoft.com/en-us/powershell/module/addsadministration/get-adgroup?view=win10-ps) cmdlet to get the SID, for example:  

   ```
   Get-ADGroup -Filter 'Name -eq "<Name of the AD Group>"'
   ```
**Note**  
\(SAML\-based federated authentication\) The group ID/name should match the group attribute information that is returned in the SAML assertion\.

1. For **Description**, enter a brief description of the authorization rule\.

1. Choose **Add authorization rule**\.

**To add an authorization rule to a Client VPN endpoint \(AWS CLI\)**  
Use the [authorize\-client\-vpn\-ingress](https://docs.aws.amazon.com/cli/latest/reference/ec2/authorize-client-vpn-ingress.html) command\.

## Remove an authorization rule from a Client VPN endpoint<a name="cvpn-working-rule-revoke"></a>

By deleting an authorization rule, you remove access to the specified network\. 

You can remove authorization rules from a Client VPN endpoint using the console and the AWS CLI\.

**To remove an authorization rule from a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to which the authorization rule is added and choose **Authorization**\.

1. Select the authorization rule to delete, choose **Revoke ingress**, and choose **Revoke ingress**\.

**To remove an authorization rule from a Client VPN endpoint \(AWS CLI\)**  
Use the [revoke\-client\-vpn\-ingress](https://docs.aws.amazon.com/cli/latest/reference/ec2/revoke-client-vpn-ingress.html) command\.

## View authorization rules<a name="cvpn-working-rule-view"></a>

You can view authorization rules for a specific Client VPN endpoint using the console and the AWS CLI\.

**To view authorization rules \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint for which to view authorization rules and choose **Authorization**\.

**To view authorization rules \(AWS CLI\)**  
Use the [describe\-client\-vpn\-authorization\-rules](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-authorization-rules.html) command\.