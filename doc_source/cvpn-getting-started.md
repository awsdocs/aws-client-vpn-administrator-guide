# Getting started with Client VPN<a name="cvpn-getting-started"></a>

The following tasks help you become familiar with Client VPN\. In this tutorial, you will create a Client VPN endpoint that does the following:
+ Provides all clients access to a single VPC\.
+ Provides all clients access to the internet\.
+ Uses [mutual authentication](client-authentication.md#mutual)\.

The following diagram represents the configuration of your VPC and Client VPN endpoint after you've completed this tutorial\.

![\[Client VPN accessing the internet\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-scenario-igw.png)

**Topics**
+ [Prerequisites](#cvpn-getting-started-prereq)
+ [Step 1: Generate server and client certificates and keys](#cvpn-getting-started-certs)
+ [Step 2: Create a Client VPN endpoint](#cvpn-getting-started-endpoint)
+ [Step 3: Enable VPN connectivity for clients](#cvpn-getting-started-target)
+ [Step 4: Authorize clients to access a network](#cvpn-getting-started-rules)
+ [Step 5: \(Optional\) Enable access to additional networks](#cvpn-getting-started-routes)
+ [Step 6: Download the Client VPN endpoint configuration file](#cvpn-getting-started-config)
+ [Step 7: Connect to the Client VPN endpoint](#cvpn-getting-started-connect)

## Prerequisites<a name="cvpn-getting-started-prereq"></a>

To complete this getting started tutorial, you need the following:
+ The permissions required to work with Client VPN endpoints\.
+ A VPC with at least one subnet and an internet gateway\. The route table that's associated with your subnet must have a route to the internet gateway\.

## Step 1: Generate server and client certificates and keys<a name="cvpn-getting-started-certs"></a>

This tutorial uses mutual authentication\. With mutual authentication, Client VPN uses certificates to perform authentication between the client and the server\.

For detailed steps to generate the server and client certificates and keys, see [Mutual authentication](client-authentication.md#mutual)\.

## Step 2: Create a Client VPN endpoint<a name="cvpn-getting-started-endpoint"></a>

When you create a Client VPN endpoint, you create the VPN construct to which clients can connect in order to establish a VPN connection\.

**To create a Client VPN endpoint**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints** and then choose **Create Client VPN Endpoint**\.

1. \(Optional\) Provide a name and description for the Client VPN endpoint\.

1. For **Client IPv4 CIDR**, specify an IP address range, in CIDR notation, from which to assign client IP addresses\. For example, `10.0.0.0/22`\.
**Note**  
The IP address range cannot overlap with the target network or any of the routes that will be associated with the Client VPN endpoint\. The client CIDR range must have a block size that is between /12 and /22 and not overlap with VPC CIDR or any other route in the route table\. You cannot change the client CIDR after you create the Client VPN endpoint\.

1. For **Server certificate ARN**, specify the ARN for the TLS certificate to be used by the server\. Clients use the server certificate to authenticate the Client VPN endpoint to which they are connecting\. 
**Note**  
The server certificate must be provisioned in AWS Certificate Manager \(ACM\)\.

1. Specify the authentication method to be used to authenticate clients when they establish a VPN connection\. For this tutorial, choose **Use mutual authentication**, and then for **Client certificate ARN**, specify the ARN of the client certificate that you generated in [Step 1](#cvpn-getting-started-certs)\.

1. For **Do you want to log the details on client connections?**, choose **No**\.

1. Leave the rest of the default settings, and choose **Create Client VPN Endpoint**\.

For more information about the other options that you can specify when creating a Client VPN endpoint, see [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

After you create the Client VPN endpoint, its state is `pending-associate`\. Clients can only establish a VPN connection after you associate at least one target network\.

## Step 3: Enable VPN connectivity for clients<a name="cvpn-getting-started-target"></a>

To enable clients to establish a VPN session, you must associate a target network with the Client VPN endpoint\. A target network is a subnet in a VPC\. 

**To associate a subnet with the Client VPN endpoint**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint with which to associate the subnet and choose **Associations**, **Associate**\.

1. For **VPC**, choose the VPC in which the subnet is located\. If you specified a VPC when you created the Client VPN endpoint, it must be the same VPC\.

1. For **Subnet to associate**, choose the subnet to associate with the Client VPN endpoint\.

1. Choose **Associate**\.
**Note**  
If authorization rules allow it, one subnet association is enough for clients to access a VPC's entire network\. You can associate additional subnets to provide high availability in case one of the Availability Zones goes down\.

When you associate the first subnet with the Client VPN endpoint, the following happens:
+ The state of the Client VPN endpoint changes to `available`\. Clients can now establish a VPN connection, but they cannot access any resources in the VPC until you add the authorization rules\.
+ The local route of the VPC is automatically added to the Client VPN endpoint route table\. 
+ The VPC's default security group is automatically applied for the subnet association\.

## Step 4: Authorize clients to access a network<a name="cvpn-getting-started-rules"></a>

To authorize clients to access the VPC in which the associated subnet is located, you must create an authorization rule\. The authorization rule specifies which clients have access to the VPC\. In this tutorial, you grant access to all users\.

**To add an authorization rule to the target network**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to which to add the authorization rule, choose **Authorization**, and then choose **Authorize Ingress**\.

1. For **Destination network to enable**, enter CIDR of the network for which you want to allow access\. For example, to allow access to the entire VPC, specify the IPv4 CIDR block of the VPC\.

1. For **Grant access to**, choose **Allow access to all users**\.

1. For **Description**, enter a brief description of the authorization rule\.

1. Choose **Add authorization rule**\.

1. Ensure that the security groups for the resources in your VPC have a rule that allows access from the security group for the [subnet association](#cvpn-getting-started-target)\. This enables your clients to access the resources in your VPC\. For more information, see [Security groups](client-authorization.md#security-groups)\.

## Step 5: \(Optional\) Enable access to additional networks<a name="cvpn-getting-started-routes"></a>

You can enable access to additional networks connected to the VPC, such as AWS services, peered VPCs, and on\-premises networks\. For each additional network, you must add a route to the network and configure an authorization rule to give clients access\.

In this tutorial, add a route to the internet \(`0.0.0.0/0`\) and add an authorization rule that grants access to all users\.

**To enable access to the internet**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to which to add the route, choose **Route Table**, and then choose **Create Route**\.

1. For **Route destination**, enter `0.0.0.0/0`\. For **Target VPC Subnet ID**, specify the ID of the subnet through which to route traffic\.

1. Choose **Create Route**\.

1. Choose **Authorization**, and then choose **Authorize Ingress**\.

1. For **Destination network to enable**, enter `0.0.0.0/0`, and choose **Allow access to all users**\.

1. Choose **Add authorization rule**\.

1. Ensure that the security group that's associated with subnet you are routing traffic through allows inbound and outbound traffic to and from the internet\. To do this, add inbound and outbound rules that allow internet traffic to and from `0.0.0.0/0`\.

## Step 6: Download the Client VPN endpoint configuration file<a name="cvpn-getting-started-config"></a>

The final step is to download and prepare the Client VPN endpoint configuration file\. The configuration file includes the Client VPN endpoint and certificate information required to establish a VPN connection\. You must provide this file to the clients who need to connect to the Client VPN endpoint to establish a VPN connection\. The client uploads this file into their VPN client application\. 

**To download and prepare the Client VPN endpoint configuration file**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint and choose **Download Client Configuration**\.

1. Locate the client certificate and key that were generated in [Step 1](#cvpn-getting-started-certs)\. The client certificate and key can be found in the following locations in the cloned OpenVPN easy\-rsa repo: 
   + Client certificate — `easy-rsa/easyrsa3/pki/issued/client1.domain.tld.crt`
   + Client key — `easy-rsa/easyrsa3/pki/private/client1.domain.tld.key`

1. Open the Client VPN endpoint configuration file using your preferred text editor and add the contents of the client certificate between `<cert>``</cert>` tags and the contents of the private key between `<key>``</key>` tags\.

   ```
   <cert>
   Contents of client certificate (.crt) file
   </cert>
   
   <key>
   Contents of private key (.key) file
   </key>
   ```

1. Prepend a random string to the Client VPN endpoint DNS name\. Locate the line that specifies the Client VPN endpoint DNS name, and prepend a random string to it so that the format is *random\_string\.displayed\_DNS\_name*\. For example:
   + Original DNS name: `cvpn-endpoint-0102bc4c2eEXAMPLE.prod.clientvpn.us-west-2.amazonaws.com`
   + Modified DNS name: `asdfa.cvpn-endpoint-0102bc4c2eEXAMPLE.prod.clientvpn.us-west-2.amazonaws.com`

1. Save and close the Client VPN endpoint configuration file\.

1. Distribute the Client VPN endpoint configuration file to your clients\.

For more information about the Client VPN endpoint configuration file, see [Export and configure the client configuration file](cvpn-working-endpoints.md#cvpn-working-endpoint-export)\.

## Step 7: Connect to the Client VPN endpoint<a name="cvpn-getting-started-connect"></a>

You can connect to the Client VPN endpoint using the AWS\-provided client or another OpenVPN\-based client application\. For more information, see the [AWS Client VPN User Guide](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/)\.