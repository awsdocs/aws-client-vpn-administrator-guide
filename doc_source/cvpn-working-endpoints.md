# Client VPN endpoints<a name="cvpn-working-endpoints"></a>

All client VPN sessions terminate at the Client VPN endpoint\. You configure the Client VPN endpoint to manage and control all client VPN sessions\. 

**Topics**
+ [Create a Client VPN endpoint](#cvpn-working-endpoint-create)
+ [Modify a Client VPN endpoint](#cvpn-working-endpoint-modify)
+ [View Client VPN endpoints](#cvpn-working-endpoint-view)
+ [Delete a Client VPN endpoint](#cvpn-working-endpoint-delete)

## Create a Client VPN endpoint<a name="cvpn-working-endpoint-create"></a>

Create a Client VPN endpoint to enable your clients to establish a VPN session\.

The Client VPN must be created in the same AWS account in which the intended target network is provisioned\.

**Prerequisites**  
Before you begin, ensure that you do the following:
+ Review the rules and limitations in [Limitations and rules of Client VPN](what-is.md#what-is-limitations)\.
+ Generate the server certificate, and if required, the client certificate\. For more information, see [Client authentication](client-authentication.md)\.

**To create a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints** and then choose **Create Client VPN Endpoint**\.

1. \(Optional\) Provide a name tag and description for the Client VPN endpoint\.

1. For **Client IPv4 CIDR**, specify an IP address range, in CIDR notation, from which to assign client IP addresses\. For example, `10.0.0.0/22`\.
**Note**  
The address range cannot overlap with the target network address range, the VPC address range or any of the routes that will be associated with the Client VPN endpoint\. The client address range must be at minimum /22 and not greater than /12 CIDR block size\. You cannot change the client address range after you create the Client VPN endpoint\.

1. For **Server certificate ARN**, specify the ARN for the TLS certificate to be used by the server\. Clients use the server certificate to authenticate the Client VPN endpoint to which they are connecting\.
**Note**  
The server certificate must be present in AWS Certificate Manager \(ACM\) in the region you are creating the Client VPN endpoint\. The certificate can either be provisioned with ACM or imported into ACM\.

1. Specify the authentication method to be used to authenticate clients when they establish a VPN connection\. You must select an authentication method\.
   + To use user\-based authentication, select **Use user\-based authentication**, and then choose one of the following:
     + **Active Directory authentication**: Choose this option for Active Directory authentication\. For **Directory ID**, specify the ID of the Active Directory to use\.
     + **Federated authentication**: Choose this option for SAML\-based federated authentication\. 

       For **SAML provider ARN**, specify the ARN of the IAM SAML identity provider\. 

       \(Optional\) For **Self\-service SAML provider ARN**, specify the ARN of the IAM SAML identity provider that you created to [support the self\-service portal](client-authentication.md#saml-self-service-support), if applicable\.
   + To use mutual certificate authentication, select **Use mutual authentication**, and then for **Client certificate ARN**, specify the ARN of the client certificate that's provisioned in AWS Certificate Manager \(ACM\)\.
**Note**  
If the server and client certificates have been issued by the same Certificate Authority \(CA\), you can use the server certificate ARN for both server and client\. If the client certificate was issued by a different CA, then the client certificate ARN should be specified\.

1. \(Optional\) For **Connection logging**, specify whether to log data about client connections using Amazon CloudWatch Logs\. Turn on **Enable log details on client connections**\. For **CloudWatch Logs log group name**, enter the name of the log group to use\. For **CloudWatch Logs log stream name**, enter the name of the log stream to use, or leave this option blank to let us create a log stream for you\. 

1. \(Optional\) For **Client Connect Handler**, turn on **Enable client connect handler** to run custom code that allows or denies a new connection to the Client VPN endpoint\. For **Client Connect Handler ARN**, specify the Amazon Resource Name \(ARN\) of the Lambda function that contains the logic that allows or denies connections\.

1. \(Optional\) Specify which DNS servers to use for DNS resolution\. To use custom DNS servers, for **DNS Server 1 IP address** and **DNS Server 2 IP address**, specify the IP addresses of the DNS servers to use\. To use VPC DNS server, for either **DNS Server 1 IP address** or **DNS Server 2 IP address**, specify the IP addresses, and add the VPC DNS server IP address\.
**Note**  
Verify that the DNS servers can be reached by clients\.

1. \(Optional\) By default, the Client VPN endpoint uses the `UDP` transport protocol\. To use the `TCP` transport protocol instead, for **Transport Protocol**, select **TCP**\.
**Note**  
UDP typically offers better performance than TCP\. You cannot change the transport protocol after you create the Client VPN endpoint\.

1. \(Optional\) To have the endpoint be a split\-tunnel Client VPN endpoint, turn on **Enable split\-tunnel**\. By default, split\-tunnel on a Client VPN endpoint is disabled\.

1. \(Optional\) For **VPC ID**, choose the VPC to associate with the Client VPN endpoint\. For **Security Group IDs**, choose one or more of the VPC's security groups to apply to the Client VPN endpoint\.

1. \(Optional\) For **VPN port**, choose the VPN port number\. The default is 443\.

1. \(Optional\) To generate a [self\-service portal URL](cvpn-self-service-portal.md) for clients, turn on **Enable self\-service portal**\.

1. \(Optional\) For **Session timeout hours**, choose the desired maximum VPN session duration time in hours from the available options, or leave set to default of 24 hours\.

1. \(Optional\) Specify whether to enable client login banner text\. Turn on **Enable client login banner**\. For **Client login banner text**, enter the text that will be displayed in a banner on AWS provided clients when a VPN session is established\. UTF\-8 encoded characters only\. Maximum of 1400 characters\.

1. Choose **Create Client VPN endpoint**\.

After you create the Client VPN endpoint, do the following to complete the configuration and enable clients to connect:
+ The initial state of the Client VPN endpoint is `pending-associate`\. Clients can only connect to the Client VPN endpoint after you associate the first [target network](cvpn-working-target.md#cvpn-working-target-associate)\.
+ Create an [authorization rule](cvpn-working-rules.md) to specify which clients have access to the network\.
+ Download and prepare the Client VPN endpoint [configuration file](cvpn-working-endpoint-export.md) to distribute to your clients\.
+ Instruct your clients to use the AWS provided client or another OpenVPN\-based client application to connect to the Client VPN endpoint\. For more information, see the [AWS Client VPN User Guide](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/)\.

**To create a Client VPN endpoint \(AWS CLI\)**  
Use the [create\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-client-vpn-endpoint.html) command\.

## Modify a Client VPN endpoint<a name="cvpn-working-endpoint-modify"></a>

After a Client VPN has been created, you can modify any of the following settings: 
+ The description
+ The server certificate
+ The client connection logging options
+ The client connect handler option
+ The DNS servers
+ The split\-tunnel option
+ Routes \(when using the split\-tunnel option\)
+ Certificate Revocation List \(CRL\)
+ Authorization rules
+ The VPC and security group associations
+ The VPN port number
+ The self\-service portal option
+ The maximum VPN session duration
+ Enable or disable client login banner text
+ Client login banner text

**Note**  
Modifications to Client VPN endpoints, including Certificate Revocation List \(CRL\) changes, will take effect up to 4 hours after a request is accepted by the Client VPN service\.  
You cannot modify the client IPv4 CIDR range, authentication options, client certificate or transport protocol after the Client VPN endpoint has been created\.

When you modify any of the following parameters on a Client VPN endpoint, the connection resets:
+ The server certificate
+ The DNS servers
+ The split\-tunnel option \(turning support on or off\)
+ Routes \(when you use the split\-tunnel option\)
+  Certificate Revocation List \(CRL\)
+ Authorization rules
+ The VPN port number

You can modify a Client VPN endpoint by using the console or the AWS CLI\. 

**To modify a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to modify, choose **Actions**, and then choose **Modify Client VPN endpoint**\.

1. For **Description**, enter a brief description for the Client VPN endpoint\.

1. For **Server certificate ARN**, specify the ARN for the TLS certificate to be used by the server\. Clients use the server certificate to authenticate the Client VPN endpoint to which they are connecting\.
**Note**  
The server certificate must be present in AWS Certificate Manager \(ACM\) in the region you are creating the Client VPN endpoint\. The certificate can either be provisioned with ACM or imported into ACM\.

1. Specify whether to log data about client connections using Amazon CloudWatch Logs\. For **Enable log details on client connections**, do one of the following:
   + To activate client connection logging, turn on **Enable log details on client connections**\. For **CloudWatch Logs log group name**, select the name of the log group to use\. For **CloudWatch Logs log stream name**, select the name of the log stream to use, or leave this option blank to let us create a log stream for you\.
   + To deactivate client connection logging, turn off **Enable log details on client connections**\.

1. For **Client connect handler**, to activate the [client connect handler](connection-authorization.md) turn on **Enable client connect handler**\. For **Client Connect Handler ARN**, specify the Amazon Resource Name \(ARN\) of the Lambda function that contains the logic that allows or denies connections\.

1. Turn on or off **Enable DNS servers**\. To use custom DNS servers, for **DNS Server 1 IP address** and **DNS Server 2 IP address**, specify the IP addresses of the DNS servers to use\. To use VPC DNS server, for either **DNS Server 1 IP address** or **DNS Server 2 IP address**, specify the IP addresses, and add the VPC DNS server IP address\.
**Note**  
Verify that the DNS servers can be reached by clients\.

1. Turn on or off **Enable split\-tunnel**\. By default, split\-tunnel on a VPN endpoint is off\.

1. For **VPC ID**, choose the VPC to associate with the Client VPN endpoint\. For **Security Group IDs**, choose one or more of the VPC's security groups to apply to the Client VPN endpoint\.

1. For **VPN port**, choose the VPN port number\. The default is 443\.

1. To generate a [self\-service portal URL](cvpn-self-service-portal.md) for clients, turn on **Enable self\-service portal**\.

1. For **Session timeout hours**, choose the desired maximum VPN session duration time in hours from the available options, or leave set to default of 24 hours\.

1. Turn on or off **Enable client login banner**\. If you want to use the client login banner, enter the text that will be displayed in a banner on AWS provided clients when a VPN session is established\. UTF\-8 encoded characters only\. Maximum of 1400 characters\.

1. Choose **Modify Client VPN endpoint**\.

**To modify a Client VPN endpoint \(AWS CLI\)**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command\.

## View Client VPN endpoints<a name="cvpn-working-endpoint-view"></a>

You can view information about Client VPN endpoints by using the console or the AWS CLI\.

**To view Client VPN endpoints \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to view\.

1. Use the **Details**, **Target network associations**, **Security groups**, **Authorization rules**, **Route table**, **Connections** and **Tags** tabs to view information about existing Client VPN endpoints\.

   You can also use filters to help refine your search\.

**To view Client VPN endpoints \(AWS CLI\)**  
Use the [describe\-client\-vpn\-endpoints](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-endpoints.html) command\.

## Delete a Client VPN endpoint<a name="cvpn-working-endpoint-delete"></a>

You will need to disassociate all target networks before you can delete a Client VPN endpoint\. When you delete a Client VPN endpoint, its state is changed to `deleting` and clients can no longer connect to it\. 

You can delete a Client VPN endpoint by using the console or the AWS CLI\.

**To delete a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to delete\. Choose **Actions**, **Delete Client VPN endpoint**\.

1. Enter *delete* into the confirmation window and choose** Delete**\.

**To delete a Client VPN endpoint \(AWS CLI\)**  
Use the [delete\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-client-vpn-endpoint.html) command\.