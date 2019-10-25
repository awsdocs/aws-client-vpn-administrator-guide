# Identity and Access Management for Client VPN<a name="cvpn-authentication"></a>

AWS uses security credentials to identify you and to grant you access to your AWS resources\. You can use features of AWS Identity and Access Management \(IAM\) to allow other users, services, and applications to use your AWS resources fully or in a limited way, without sharing your security credentials\.

By default, IAM users don't have permission to create, view, or modify AWS resources\. To allow an IAM user to access resources, such as a Client VPN endpoint, and perform tasks, you must create an IAM policy\. This policy must grant the IAM user permission to use the specific resources and API actions they need\. Then, attach the policy to the IAM user or the group to which the IAM user belongs\. When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

For more information about IAM, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\. For a list of Amazon EC2 actions, including Client VPN actions, see [Actions, Resources, and Condition Keys for Amazon EC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonec2.html) in the *IAM User Guide*\.

For more information about authentication and authorization for connecting to a Client VPN endpoint, see [Client Authentication and Authorization](authentication-authorization.md)\.