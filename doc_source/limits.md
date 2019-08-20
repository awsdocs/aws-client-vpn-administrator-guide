# AWS Client VPN Limits<a name="limits"></a>

You can request a limit increase for some of these limits\. For some limits, you can view your current limit using the **Limits** page in the Amazon EC2 console\. To access the Limits page, open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/), and from the navigation bar, choose **Limits**\.

Your AWS account has the following limits related to Client VPN endpoints\.
+ Number of Client VPN endpoints per account: 5
+ Number of authorization rules per Client VPN endpoint: 50
+ Number of routes per Client VPN endpoint: 10
+ Number of concurrent client connections per Client VPN endpoint: 2000
+ Number of concurrent operations per Client VPN endpoint: 10

  Operations include:
  + Associating or disassociating subnets
  + Creating or deleting routes
  + Creating or deleting inbound and outbound rules
  + Creating or deleting security groups

Take the following into consideration when you use Client VPN endpoints\.
+ The Client VPN endpoint must belong to the same account as the VPC that contains the subnet that you want to associate with the Client VPN endpoint\. 
+ If you use Active Directory to authenticate the user, the Client VPN endpoint must belong to the same account as the AWS Directory Service resource used for Active Directory authentication\.