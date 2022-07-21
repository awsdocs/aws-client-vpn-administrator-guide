# Getting started with Client VPN<a name="cvpn-getting-started"></a>

In this tutorial you will create a Client VPN endpoint that does the following:
+ Provides all clients with access to a single VPC\.
+ Provides all clients with access to the internet\.
+ Uses [mutual authentication](client-authentication.md#mutual)\.

The following diagram represents the configuration of your VPC and Client VPN endpoint after you've completed this tutorial\.

![\[Client VPN accessing the internet\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-scenario-igw.png)

**Topics**
+ [Prerequisites](#cvpn-getting-started-prereq)
+ [Step 1: Generate server and client certificates and keys](#cvpn-getting-started-certs)
+ [Step 2: Create a Client VPN endpoint](#cvpn-getting-started-endpoint)
+ [Step 3: Associate a target network](#cvpn-getting-started-target)
+ [Step 4: Add an authorization rule for the VPC](#cvpn-getting-started-rules)
+ [Step 5: Provide access to the internet](#cvpn-getting-started-routes)
+ [Step 6: Verify security group requirements](#cvpn-getting-started-sec-groups)
+ [Step 7: Download the Client VPN endpoint configuration file](#cvpn-getting-started-config)
+ [Step 8: Connect to the Client VPN endpoint](#cvpn-getting-started-connect)

## Prerequisites<a name="cvpn-getting-started-prereq"></a>

Before you begin this getting started tutorial, make sure that you have the following:
+ The permissions required to work with Client VPN endpoints\.
+ The permissions required to import certificates into AWS Certificate Manager\.
+ A VPC with at least one subnet and an internet gateway\. The route table that's associated with your subnet must have a route to the internet gateway\.

## Step 1: Generate server and client certificates and keys<a name="cvpn-getting-started-certs"></a>

This tutorial uses mutual authentication\. With mutual authentication, Client VPN uses certificates to perform authentication between clients and the Client VPN endpoint\. You will need to have a server certificate and key, and at least one client certificate and key\. At minimum, the server certificate will need to be imported into AWS Certificate Manager \(ACM\) and specified when you create the Client VPN endpoint\. Importing the client certificate into ACM is optional\.

If you don't already have certificates to use for this purpose, they can be created using the OpenVPN easy\-rsa utility\. For detailed steps to generate the server and client certificates and keys using the [OpenVPN easy\-rsa utility](https://github.com/OpenVPN/easy-rsa), and import them into ACM see [Mutual authentication](client-authentication.md#mutual)\.

## Step 2: Create a Client VPN endpoint<a name="cvpn-getting-started-endpoint"></a>

The Client VPN endpoint is the resource that you create and configure to enable and manage client VPN sessions\. It's the termination point for all client VPN sessions\.

**To create a Client VPN endpoint**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints** and then choose **Create Client VPN endpoint**\.

1. \(Optional\) Provide a name tag and description for the Client VPN endpoint\.

1. For **Client IPv4 CIDR**, specify an IP address range, in CIDR notation, from which to assign client IP addresses\. For example, `10.0.0.0/22`\.
**Note**  
The address range cannot overlap with the target network address range, the VPC address range, or any of the routes that will be associated with the Client VPN endpoint\. The client address range must be at minimum /22 and not greater than /12 CIDR block size\. You cannot change the client address range after you create the Client VPN endpoint\.

1. For **Server certificate ARN**, select the ARN of the server certificate that you generated in [Step 1](#cvpn-getting-started-certs)\.
**Note**  
The server certificate must be provisioned with or imported into AWS Certificate Manager \(ACM\) in the same AWS Region\.

1. Under **Authentication options**, choose **Use mutual authentication**, and then for **Client certificate ARN**, select the ARN of the certificate you want to use as the client certificate\.
**Note**  
If the server and client certificates are signed by the same certificate authority \(CA\), you have the option of specifying the server certificate ARN for *both* the client and server certificates\. In this scenario, any client certificate that corresponds with the server certificate can be used to authenticate\.

1. Keep the rest of the default settings, and choose **Create Client VPN endpoint**\.

After you create the Client VPN endpoint, its state is `pending-associate`\. Clients can only establish a VPN connection after you associate at least one target network\.

For more information about the other options that you can specify when creating a Client VPN endpoint, see [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)\.

## Step 3: Associate a target network<a name="cvpn-getting-started-target"></a>

To allow clients to establish a VPN session, you associate a target network with the Client VPN endpoint\. A target network is a subnet in a VPC\. 

**To associate a target network with the Client VPN endpoint**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint that you created in the preceding procedure, and then choose **Target network associations**, **Associate target network**\.

1. For **VPC**, choose the VPC in which the subnet is located\. 

1. For **Choose a subnet to associate**, choose the subnet to associate with the Client VPN endpoint\.

1. Choose **Associate target network**\.
**Note**  
If authorization rules allow it, one subnet association is enough for clients to access a VPC's entire network\. You can associate additional subnets to provide high availability in case one of the Availability Zones goes down\.

When you associate the first subnet with the Client VPN endpoint, the following happens:
+ The state of the Client VPN endpoint changes to `available`\. Clients can now establish a VPN connection, but they cannot access any resources in the VPC until you add the authorization rules\.
+ The local route of the VPC is automatically added to the Client VPN endpoint route table\. 
+ The VPC's default security group is automatically applied for the Client VPN endpoint\.

## Step 4: Add an authorization rule for the VPC<a name="cvpn-getting-started-rules"></a>

For clients to access the VPC, there needs to be a route to the VPC in the Client VPN endpoint's route table and an authorization rule\. The route was already added automatically in the previous step\. For this tutorial, we want to grant all users access to the VPC\.

**To add an authorization rule for the VPC**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to which to add the authorization rule\. Choose **Authorization rules**, and then choose **Add authorization rule**\.

1. For **Destination network to enable access**, enter the CIDR of the network for which you want to allow access\. For example, to allow access to the entire VPC, specify the IPv4 CIDR block of the VPC\.

1. For **Grant access to**, choose **Allow access to all users**\.

1. \(Optional\) For **Description**, enter a brief description of the authorization rule\.

1. Choose **Add authorization rule**\.

## Step 5: Provide access to the internet<a name="cvpn-getting-started-routes"></a>

You can provide access to additional networks connected to the VPC, such as AWS services, peered VPCs, on\-premises networks, and the internet\. For each additional network, you add a route to the network in the Client VPN endpoint's route table and configure an authorization rule to give clients access\.

For this tutorial, we want to grant all users access to the internet and also to the VPC\. You've already configured access to the VPC, so this step is for access to the internet\.

**To provide access to the internet**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint that you created for this tutorial\. Choose **Route Table**, and then choose **Create Route**\.

1. For **Route destination**, enter `0.0.0.0/0`\. For **Subnet ID for target network association**, specify the ID of the subnet through which to route traffic\.

1. Choose **Create Route**\.

1. Choose **Authorization rules**, and then choose **Add authorization rule**\.

1. For **Destination network to enable access**, enter `0.0.0.0/0`, and choose **Allow access to all users**\.

1. Choose **Add authorization rule**\.

## Step 6: Verify security group requirements<a name="cvpn-getting-started-sec-groups"></a>

In this tutorial, no security groups were specified during the creation of the Client VPN endpoint in Step 2\. That means that the default security group for the VPC is automatically applied to the Client VPN endpoint when a target network is associated\. As a result, the default security group for the VPC should now be associated with the Client VPN endpoint\.

**Verify the following security group requirements**
+ That the security group associated with subnet you are routing traffic through \(in this case the default VPC security group\) allows outbound traffic to the internet\. To do this, add an outbound rule that allows all traffic to destination `0.0.0.0/0`\.
+ That the security groups for the resources in your VPC have a rule that allows access from the security group that's applied to the Client VPN endpoint \(in this case the default VPC security group\)\. This enables your clients to access the resources in your VPC\. 

For more information, see [Security groups](client-authorization.md#security-groups)\.

## Step 7: Download the Client VPN endpoint configuration file<a name="cvpn-getting-started-config"></a>

The next step is to download and prepare the Client VPN endpoint configuration file\. The configuration file includes the Client VPN endpoint details and certificate information required to establish a VPN connection\. You provide this file to the end users who need to connect to the Client VPN endpoint\. The end user uses the file to configure their VPN client application\. 

**To download and prepare the Client VPN endpoint configuration file**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint that you created for this tutorial, and choose **Download client configuration**\.

1. Locate the client certificate and key that were generated in [Step 1](#cvpn-getting-started-certs)\. The client certificate and key can be found in the following locations in the cloned OpenVPN easy\-rsa repo: 
   + Client certificate — `easy-rsa/easyrsa3/pki/issued/client1.domain.tld.crt`
   + Client key — `easy-rsa/easyrsa3/pki/private/client1.domain.tld.key`

1. Open the Client VPN endpoint configuration file using your preferred text editor\. Add `<cert>``</cert>` and `<key>``</key>` tags to the file\. Place the contents of the client certificate and the contents of the private key between the corresponding tags, as such:

   ```
   <cert>
   Contents of client certificate (.crt) file
   </cert>
   
   <key>
   Contents of private key (.key) file
   </key>
   ```

1. Locate the line that specifies the Client VPN endpoint DNS name, and prepend a random string to it so that the format is *random\_string\.displayed\_DNS\_name*\. For example:
   + Original DNS name: `cvpn-endpoint-0102bc4c2eEXAMPLE.prod.clientvpn.us-west-2.amazonaws.com`
   + Modified DNS name: `asdfa.cvpn-endpoint-0102bc4c2eEXAMPLE.prod.clientvpn.us-west-2.amazonaws.com`
**Note**  
We recommend that you always use the DNS name provided for the Client VPN endpoint in your configuration file, as described\. The IP addresses that the DNS name will resolve to are subject to change\. 

1. Save and close the Client VPN endpoint configuration file\.

1. Distribute the Client VPN endpoint configuration file to your end users\.

For more information about the Client VPN endpoint configuration file, see [Export and configure the client configuration file](cvpn-working-endpoint-export.md)\.

## Step 8: Connect to the Client VPN endpoint<a name="cvpn-getting-started-connect"></a>

You can connect to the Client VPN endpoint using the AWS provided client or another OpenVPN\-based client application and the configuration file that you just created\. For more information, see the [AWS Client VPN User Guide](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/)\.