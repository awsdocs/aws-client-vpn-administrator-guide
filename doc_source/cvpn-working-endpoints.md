# Client VPN Endpoints<a name="cvpn-working-endpoints"></a>

All client VPN sessions terminate at the Client VPN endpoint\. You configure the Client VPN endpoint to manage and control all client VPN sessions\. 

**Topics**
+ [Create a Client VPN Endpoint](#cvpn-working-endpoint-create)
+ [Modify a Client VPN Endpoint](#cvpn-working-endpoint-modify)
+ [Export Client Configuration](#cvpn-working-endpoint-export)
+ [View Client VPN Endpoints](#cvpn-working-endpoint-view)
+ [Delete a Client VPN Endpoint](#cvpn-working-endpoint-delete)

## Create a Client VPN Endpoint<a name="cvpn-working-endpoint-create"></a>

You must create a Client VPN endpoint to enable your clients to establish a VPN session\. After you create a new Client VPN endpoint, its status is `pending-associate`\. Clients can only connect to the Client VPN endpoint after you associate the first target network\.

The Client VPN must be created in the same AWS account in which the intended target network is provisioned\.

Make sure that you have the client certificate and the client private key before you add a Client VPN endpoint\.

You can create a Client VPN endpoint by using the console or the AWS CLI\.

**To create a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints** and choose **Create Client VPN Endpoint\.**

1. \(Optional\) For **Description**, enter a brief description for the Client VPN endpoint\.

1. For **Client IPv4 CIDR**, specify an IP address range, in CIDR notation, from which to assign client IP addresses\.
**Note**  
The IP address range cannot overlap with the target network or with any of the routes that will be associated with the Client VPN endpoint\. The client CIDR range must have a block size of at least /22, and it must not be greater than /16\.
**Important**  
The IP address range cannot be changed after the Client VPN endpoint has been created\. 

1. For **Server certificate ARN**, specify the ARN for the TLS certificate to be used by the server\. Clients use the server certificate to authenticate the Client VPN endpoint to which they are connecting\.
**Note**  
The server certificate must be provisioned in AWS Certificate Manager \(ACM\)\.

1. Specify the authentication method to be used to authenticate clients when they establish a VPN connection\. You must select at least one authentication method\.
   + To use Active Directory authentication, select **Use Active Directory authentication**, and then for **Directory ID**, specify the ID of the Active Directory to use\.
   + To use mutual certificate authentication, select **Use mutual authentication**, and then for **Client certificate ARN**, specify the ARN of the client certificate\.
**Note**  
If the client certificate has been issued by the same Certificate Authority \(Issuer\) as the server certificate, then you can continue to use the server certificate ARN for the client certificate ARN\. The client certificate must be provisioned in AWS Certificate Manager \(ACM\)\.

1. Specify whether to log data about client connections using Amazon CloudWatch Logs\. For **Do you want to log the details on clients connections?**, do one of the following:
   + To enable client connection logging, choose **Yes**, for **CloudWatch Logs log group name** enter the name of the log group to use, and for **CloudWatch Logs log stream name**, enter the name of the log stream to use\.
   + To disable client connection logging, choose **No**\.

1. Specify which DNS servers to use for DNS resolution\. To use custom DNS servers, for **DNS Server 1 IP address** and **DNS Server 2 IP address**, specify the IP addresses of the DNS servers to use\. To use VPC DNS server, for either **DNS Server 1 IP address** or **DNS Server 2 IP address**, specify the IP addresses, and add the VPC DNS server IP address\.
**Note**  
Ensure that the DNS servers can be reached by clients\.

1. \(Optional\) To have the endpoint be a split\-tunnel VPN endpoint, select **Enable split\-tunnel**\.

   By default, split\-tunnel on a VPN endpoint is disabled\.

1. \(Optional\) By default, the Client VPN server uses the `UDP` transport protocol\. To use the `TCP` transport protocol instead, for **Transport Protocol**, select **TCP**\.
**Note**  
UDP typically offers better performance than TCP\.

1. Choose **Create Client VPN Endpoint**\.

**To create a Client VPN endpoint \(AWS CLI\)**  
Use the [create\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-client-vpn-endpoint.html) command\.

## Modify a Client VPN Endpoint<a name="cvpn-working-endpoint-modify"></a>

You can modify the server certificate, client connection logging options, DNS servers, and the description after the Client VPN endpoint has been created\. You cannot modify the client IPv4 CIDR range, authentication options, or transport protocol after the Client VPN endpoint has been created\.

You can modify a Client VPN endpoint by using the console or the AWS CLI\. 

**To modify a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to modify, choose **Actions**, and then choose **Modify Client VPN Endpoint**\.

1. Make the required changes and choose **Modify Client VPN Endpoint**\.

**To modify a Client VPN endpoint \(AWS CLI\)**  
Use the [modify\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-client-vpn-endpoint.html) command\.

## Export Client Configuration<a name="cvpn-working-endpoint-export"></a>

The Client VPN endpoint configuration file is the file clients use to establish a VPN connection with the Client VPN endpoint\. You must download this file and distribute it to all clients who need access to the VPN\.

 If you chose to use Mutual Authentication when you created the Client VPN endpoint, then you need to add the client certificate and the client private key \(by using the <cert></cert> tag and the <key></key tag\)\) to the \.ovpm configuration file that you downloaded\. After you add the information, you can import the \.ovpn file into the OpenVPN client software\. 

By default, the “\-\-remote\-random\-hostname” option in the OpenVPN client configuration enables wild card DNS\. Because wild card DNS is enabled, the client does not cache the IP address of the endpoint and you will not be able to ping the DNS name of the endpoint\. 

You can export the client configuration by using the console or the AWS CLI\.

**To export client configuration \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint for which to download the client configuration and choose **Download Client Configuration**\.

**To export client configuration \(AWS CLI\)**  
Use the [export\-client\-vpn\-client\-configuration](https://docs.aws.amazon.com/cli/latest/reference/ec2/export-client-vpn-client-configuration.html) command and specify the output file name\.

```
$ aws ec2 export-client-vpn-client-configuration --client-vpn-endpoint-id endpoint_id --output text>config_filename.ovpn
```

## View Client VPN Endpoints<a name="cvpn-working-endpoint-view"></a>

You can view information about Client VPN endpoints by using the console or the AWS CLI\.

**To view Client VPN endpoints using the console**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to view\.

1. Use tabs to view the associated target networks, authorization rules, routes, and client connections\.

**To view Client VPN endpoints using the AWS CLI**  
Use the [describe\-client\-vpn\-endpoints](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-endpoints.html) command\.

## Delete a Client VPN Endpoint<a name="cvpn-working-endpoint-delete"></a>

When you delete a Client VPN endpoint, its state is changed to `deleting` and clients can no longer connect to it\. You must disassociate all associated target networks before you can delete a Client VPN endpoint\.

You can delete a Client VPN endpoint by using the console or the AWS CLI\.

**To delete a Client VPN endpoint \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to delete, choose **Actions**, choose **Delete Client VPN Endpoint**, and then **Yes, Delete**\.

**To delete a Client VPN endpoint \(AWS CLI\)**  
Use the [delete\-client\-vpn\-endpoint](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-client-vpn-endpoint.html) command\.