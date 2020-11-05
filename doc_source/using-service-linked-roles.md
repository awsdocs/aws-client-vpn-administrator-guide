# Using service\-linked roles for Client VPN<a name="using-service-linked-roles"></a>

AWS Client VPN uses service\-linked roles for the permissions that it requires to call other AWS services on your behalf\. For more information, see [ Using Service\-Linked Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) in the *IAM User Guide*\.

## Service\-linked role permissions for Client VPN<a name="slr-permissions"></a>

AWS Client VPN uses the service\-linked role named **AWSServiceRoleForClientVPN** to call the following actions on your behalf when you work with Client VPN endpoints:
+ `ec2:CreateNetworkInterface`
+ `ec2:CreateNetworkInterfacePermission`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DescribeVpcs`
+ `ec2:DescribeSubnets`
+ `ec2:DescribeInternetGateways`
+ `ec2:ModifyNetworkInterfaceAttribute`
+ `ec2:DeleteNetworkInterface`
+ `ec2:DescribeAccountAttributes`
+ `ds:AuthorizeApplication`
+ `ds:DescribeDirectories`
+ `ds:GetDirectoryLimits`
+ `ds:ListAuthorizedApplications`
+ `ds:UnauthorizeApplication`
+ `lambda:GetFunctionConfiguration`
+ `logs:DescribeLogStreams`
+ `logs:CreateLogStream`
+ `logs:PutLogEvents`
+ `logs:DescribeLogGroups`
+ `acm:GetCertificate`
+ `acm:DescribeCertificate`

The **AWSServiceRoleForClientVPN** service\-linked role trusts the clientvpn\.amazonaws\.com principal to assume the role\.

If you use the client connect handler for your Client VPN endpoint, Client VPN uses a service\-linked role called **AWSServiceRoleForClientVPNConnections** to call the `lambda:InvokeFunction` action on your behalf\. For more information, see [Connection authorization](connection-authorization.md)\.

## Creating service\-linked roles for Client VPN<a name="create-slr"></a>

You don't need to manually create the **AWSServiceRoleForClientVPN** or the **AWSServiceRoleForClientVPNConnections** roles\. Client VPN creates the roles for you when you create the first Client VPN endpoint in your account\.

For Client VPN to create the service\-linked roles on your behalf, you must have the required permissions\. For more information, see [ Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Editing a service\-linked role for Client VPN<a name="edit-slr"></a>

You cannot edit the **AWSServiceRoleForClientVPN** or **AWSServiceRoleForClientVPNConnections** service\-linked roles\.

## Deleting a service\-linked role for Client VPN<a name="delete-slr"></a>

If you no longer need to use Client VPN, we recommend that you delete the **AWSServiceRoleForClientVPN** and **AWSServiceRoleForClientVPNConnections** service\-linked roles\.

You must first delete the related Client VPN resources\. This ensures that you do not inadvertently remove permission to access the resources\.

Use the IAM console, the IAM CLI, or the IAM API to delete the service\-linked roles\. For more information, see [ Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.