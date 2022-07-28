# What is AWS Client VPN?<a name="what-is"></a>

AWS Client VPN is a managed client\-based VPN service that enables you to securely access your AWS resources and resources in your on\-premises network\. With Client VPN, you can access your resources from any location using an OpenVPN\-based VPN client\.



**Topics**
+ [Features of Client VPN](#what-is-features)
+ [Components of Client VPN](#what-is-components)
+ [Working with Client VPN](#what-is-access)
+ [Limitations and rules of Client VPN](#what-is-limitations)
+ [Pricing for Client VPN](#what-is-pricing)

## Features of Client VPN<a name="what-is-features"></a>

Client VPN offers the following features and functionality:
+ **Secure connections** — It provides a secure TLS connection from any location using the OpenVPN client\.
+ **Managed service** — It is an AWS managed service, so it removes the operational burden of deploying and managing a third\-party remote access VPN solution\.
+ **High availability and elasticity** — It automatically scales to the number of users connecting to your AWS resources and on\-premises resources\.
+ **Authentication** — It supports client authentication using Active Directory, federated authentication, and certificate\-based authentication\.
+ **Granular control** — It enables you to implement custom security controls by defining network\-based access rules\. These rules can be configured at the granularity of Active Directory groups\. You can also implement access control using security groups\.
+ **Ease of use** — It enables you to access your AWS resources and on\-premises resources using a single VPN tunnel\.
+ **Manageability** — It enables you to view connection logs, which provide details on client connection attempts\. You can also manage active client connections, with the ability to terminate active client connections\.
+ **Deep integration** — It integrates with existing AWS services, including AWS Directory Service and Amazon VPC\.

## Components of Client VPN<a name="what-is-components"></a>

The following are the key concepts for Client VPN:

**Client VPN endpoint**  
The Client VPN endpoint is the resource that you create and configure to enable and manage client VPN sessions\. It's the termination point for all client VPN sessions\.

**Target network**  
A target network is the network that you associate with a Client VPN endpoint\. A subnet from a VPC is a target network\. Associating a subnet with a Client VPN endpoint enables you to establish VPN sessions\. You can associate multiple subnets with a Client VPN endpoint for high availability\. All subnets must be from the same VPC\. Each subnet must belong to a different Availability Zone\.

**Route**  
Each Client VPN endpoint has a route table that describes the available destination network routes\. Each route in the route table specifies the path for traffic to specific resources or networks\.

**Authorization rules**  
An authorization rule restricts the users who can access a network\. For a specified network, you configure the Active Directory or identity provider \(IdP\) group that is allowed access\. Only users belonging to this group can access the specified network\. By default, there are no authorization rules and you must configure authorization rules to enable users to access resources and networks\. 

**Client**  
The end user connecting to the Client VPN endpoint to establish a VPN session\. End users need to download an OpenVPN client and use the Client VPN configuration file that you created to establish a VPN session\.

**Client CIDR range**  
An IP address range from which to assign client IP addresses\. Each connection to the Client VPN endpoint is assigned a unique IP address from the client CIDR range\. You choose the client CIDR range, for example, `10.2.0.0/16`\.

**Client VPN ports**  
AWS Client VPN supports ports 443 and 1194 for both TCP and UDP\. The default is port 443\.

**Client VPN network interfaces**  
When you associate a subnet with your Client VPN endpoint, we create Client VPN network interfaces in that subnet\. Traffic that's sent to the VPC from the Client VPN endpoint is sent through a Client VPN network interface\. Source network address translation \(SNAT\) is then applied, where the source IP address from the client CIDR range is translated to the Client VPN network interface IP address\.

**Connection logging**  
You can enable connection logging for your Client VPN endpoint to log connection events\. You can use this information to run forensics, analyze how your Client VPN endpoint is being used, or debug connection issues\.

**Self\-service portal**  
Client VPN provides a self\-service portal as a web page to end users to download the latest version of the AWS VPN Desktop Client and the latest version of the Client VPN endpoint configuration file, which contains the settings required to connect to their endpoint\. The Client VPN endpoint administrator can enable or disable the self\-service portal for the Client VPN endpoint\. Self\-service portal is a Global service backed by service stacks in the Asia Pacific \(Tokyo\), US East \(N\. Virginia\), and Europe \(Ireland\) Regions, and in AWS GovCloud \(US\-West\)\.

## Working with Client VPN<a name="what-is-access"></a>

You can work with Client VPN in any of the following ways:

**Amazon VPC console**  
The Amazon VPC console provides a web\-based user interface for Client VPN\. If you've signed up for an AWS account, you can sign into the [Amazon VPC](https://console.aws.amazon.com/vpc/) console and select Client VPN in the navigation pane\.

**AWS Command Line Interface \(CLI\)**  
The AWS CLI provides direct access to the Client VPN public APIs\. It is supported on Windows, macOS, and Linux\. For more information about getting started with the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For more information about the commands for Client VPN, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

**AWS Tools for Windows PowerShell**  
AWS provides commands for a broad set of AWS offerings for those who script in the PowerShell environment\. For more information about getting started with the AWS Tools for Windows PowerShell, see the [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)\. For more information about the cmdlets for Client VPN, see the [AWS Tools for Windows PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/)\.

**Query API**  
The Client VPN HTTPS Query API gives you programmatic access to Client VPN and AWS\. The HTTPS Query API lets you issue HTTPS requests directly to the service\. When you use the HTTPS API, you must include code to digitally sign requests using your credentials\. For more information, see the [AWS Client VPN actions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/OperationList-query-cvpn.html)\.

## Limitations and rules of Client VPN<a name="what-is-limitations"></a>

Client VPN has the following rules and limitations:
+ Client CIDR ranges cannot overlap with the local CIDR of the VPC in which the associated subnet is located, or any routes manually added to the Client VPN endpoint's route table\.
+ Client CIDR ranges must have a block size of at least /22 and must not be greater than /12\.
+ A portion of the addresses in the client CIDR range are used to support the availability model of the Client VPN endpoint, and cannot be assigned to clients\. Therefore, we recommend that you assign a CIDR block that contains twice the number of IP addresses that are required to enable the maximum number of concurrent connections that you plan to support on the Client VPN endpoint\.
+ The client CIDR range cannot be changed after you create the Client VPN endpoint\.
+ The subnets associated with a Client VPN endpoint must be in the same VPC\.
+ You cannot associate multiple subnets from the same Availability Zone with a Client VPN endpoint\. 
+ A Client VPN endpoint does not support subnet associations in a dedicated tenancy VPC\.
+ Client VPN supports IPv4 traffic only\. See [IPv6 Considerations](ipv6-considerations.md) for details regarding IPv6\.
+ Client VPN is not Federal Information Processing Standards \(FIPS\) compliant\.
+ If multi\-factor authentication \(MFA\) is disabled for your Active Directory, a user password cannot be in the following format\.

  ```
  SCRV1:<base64_encoded_string>:<base64_encoded_string>
  ```
+ The self\-service portal is not available for clients that authenticate using mutual authentication\.
+ It is not recommended to connect to the Client VPN endpoint by using IP addresses\. Since Client VPN is a managed service, you will occasionally see the IP addresses the DNS name resolves to change\. In addition, you will see Client VPN network interfaces deleted and recreated in your Cloud Trail logs as well and this is expected behavior\. It is recommended to connect to the Client VPN endpoint using the DNS name provided\. 
+ IP forwarding is currently disabled when using the AWS Client VPN Desktop Application\. It has been disabled since the launch of the service on December 18, 2018, in order to address an issue reported by [NIST](https://nvd.nist.gov/vuln/detail/CVE-2020-15590)\. We understand, however, that some customers may need this functionality for their services\. While we do not have a specific date at this time, we do plan to safely enable IP forwarding in an upcoming release\.
+ AWS Managed Microsoft AD replicated from primary directory located in a different region are not currently supported. Use AWS Managed Microsoft AD id which is primary in the same region of AWS Client VPN.


## Pricing for Client VPN<a name="what-is-pricing"></a>

You are charged for each endpoint association and each VPN connection on an hourly basis\. For more information, see [AWS Client VPN pricing](http://aws.amazon.com/vpn/pricing/#AWS_Client_VPN_pricing)\.

You are charged for data transfer out from Amazon EC2 to the internet\. For more information, see [Data Transfer](http://aws.amazon.com/ec2/pricing/on-demand/#Data_Transfer) on the Amazon EC2 On\-Demand Pricing age\.

If you enable connection logging for your Client VPN endpoint, you must create a CloudWatch Logs log group in your account\. Charges apply for using log groups\. For more information, see [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) \(under **Paid tier**, choose **Logs**\)\.

If you enable the client connect handler for your Client VPN endpoint, you must create and invoke a Lambda function\. Charges apply for invoking Lambda functions\. For more information, see [AWS Lambda pricing](https://aws.amazon.com/lambda/pricing/)\.
