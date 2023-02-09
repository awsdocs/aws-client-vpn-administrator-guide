# Using roles for Client VPN<a name="using-service-linked-roles-cvpn-slr"></a>

AWS Client VPN uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Client VPN\. Service\-linked roles are predefined by Client VPN and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Client VPN easier because you don’t have to manually add the necessary permissions\. Client VPN defines the permissions of its service\-linked roles, and unless defined otherwise, only Client VPN can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Client VPN resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-linked roles** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Client VPN<a name="service-linked-role-permissions-cvpn-slr"></a>

Client VPN uses the service\-linked role named **AWSServiceRoleForClientVPN** – Allow Client VPN to create and manage resources related to your VPN connections\.

The **AWSServiceRoleForClientVPN** service\-linked role trusts the following service to assume the role:
+ `clientvpn.amazonaws.com`

The role permissions policy named **ClientVPNServiceRolePolicy** allows Client VPN to complete the following actions on the specified resources:
+ Action: `ec2:CreateNetworkInterface` on `Resource: "*"`
+ Action: `ec2:CreateNetworkInterfacePermission` on `Resource: "*"`
+ Action: `ec2:DescribeSecurityGroups` on `Resource: "*"`
+ Action: `ec2:DescribeVpcs` on `Resource: "*"`
+ Action: `ec2:DescribeSubnets` on `Resource: "*"`
+ Action: `ec2:DescribeInternetGateways` on `Resource: "*"`
+ Action: `ec2:ModifyNetworkInterfaceAttribute` on `Resource: "*"`
+ Action: `ec2:DeleteNetworkInterface` on `Resource: "*"`
+ Action: `ec2:DescribeAccountAttributes` on `Resource: "*"`
+ Action: `ds:AuthorizeApplication` on `Resource: "*"`
+ Action: `ds:DescribeDirectories` on `Resource: "*"`
+ Action: `ds:GetDirectoryLimits` on `Resource: "*"`
+ Action: `ds:UnauthorizeApplication` on `Resource: "*"`
+ Action: `logs:DescribeLogStreams` on `Resource: "*"`
+ Action: `logs:CreateLogStream` on `Resource: "*"`
+ Action: `logs:PutLogEvents` on `Resource: "*"`
+ Action: `logs:DescribeLogGroups` on `Resource: "*"`
+ Action: `acm:GetCertificate` on `Resource: "*"`
+ Action: `acm:DescribeCertificate` on `Resource: "*"`
+ Action: `iam:GetSAMLProvider` on `Resource: "*"`
+ Action: `lambda:GetFunctionConfiguration` on `Resource: "*"`

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for Client VPN<a name="create-service-linked-role-cvpn-slr"></a>

You don't need to manually create a service\-linked role\. When you create the first Client VPN endpoint in your account with the AWS Management Console, the AWS CLI, or the AWS API, Client VPN creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create the first Client VPN endpoint in your account, Client VPN creates the service\-linked role for you again\. 

## Editing a service\-linked role for Client VPN<a name="edit-service-linked-role-cvpn-slr"></a>

Client VPN does not allow you to edit the AWSServiceRoleForClientVPN service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Client VPN<a name="delete-service-linked-role-cvpn-slr"></a>

If you no longer need to use Client VPN, we recommend that you delete the **AWSServiceRoleForClientVPN** service\-linked role\.

You must first delete the related Client VPN resources\. This ensures that you do not inadvertently remove permission to access the resources\.

Use the IAM console, the IAM CLI, or the IAM API to delete the service\-linked roles\. For more information, see [ Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported regions for Client VPN service\-linked roles<a name="slr-regions-cvpn-slr"></a>

Client VPN supports using service\-linked roles in all of the regions where the service is available\. For more information, see [AWS regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.