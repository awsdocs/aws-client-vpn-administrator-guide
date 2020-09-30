# Routes<a name="cvpn-working-routes"></a>

Each Client VPN endpoint has a route table that describes the available destination network routes\. Each route in the route table determines where the network traffic is directed\. You must configure authorization rules for each Client VPN endpoint route to specify which clients have access to the destination network\.

When you associate a subnet from a VPC with a Client VPN endpoint, a route for the VPC is automatically added to the Client VPN endpoint's route table\. To enable access for additional networks, such as peered VPCs, on\-premises networks, the local network \(to enable clients to communicate with each other\), or the internet, you must manually add a route to the Client VPN endpoint's route table\.

**Topics**
+ [Split\-tunnel on AWS Client VPN endpoint considerations](#split-tunnel-routes)
+ [Create an endpoint route](#cvpn-working-routes-create)
+ [View endpoint routes](#cvpn-working-routes-view)
+ [Delete an endpoint route](#cvpn-working-routes-delete)

## Split\-tunnel on AWS Client VPN endpoint considerations<a name="split-tunnel-routes"></a>

When you use split\-tunnel on an AWS Client VPN endpoint, all of the routes that are in the AWS Client VPN route tables are added to the client route table when the VPN is established\. If you add a route after the VPN is established, you must reset the connection so that the new route is sent to the client\.

We recommend that you account for the number of routes that the client device can handle before you modify the Client VPN endpoint route table\.

## Create an endpoint route<a name="cvpn-working-routes-create"></a>

When you create a route, you specify how traffic for the destination network should be directed\.

To allow clients to access the internet, add a destination `0.0.0.0/0` route\.

You can add routes to a Client VPN endpoint by using the console and the AWS CLI\.

**To create a Client VPN endpoint route \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint to which to add the route, choose **Route Table**, and then choose **Create Route**\.

1. For **Route destination**, specify the IPv4 CIDR range for the destination network\. For example:
   + To add a route for internet access, enter `0.0.0.0/0`\.
   + To add a route for a peered VPC, enter the peered VPC's IPv4 CIDR range\.
   + To add a route for an on\-premises network, enter the AWS Site\-to\-Site VPN connection's IPv4 CIDR range\.
   + To add a route for the local network, enter the client CIDR range\.

1. For **Target VPC Subnet ID**, select the subnet that is associated with the Client VPN endpoint\.

   Alternatively, if you're adding a route for the local network, select `local`\.

1. For **Description**, enter a brief description for the route\.

1. Choose **Create Route**\.

**To create a Client VPN endpoint route \(AWS CLI\)**  
Use the [create\-client\-vpn\-route](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-client-vpn-route.html) command\.

## View endpoint routes<a name="cvpn-working-routes-view"></a>

You can view the routes for a specific Client VPN endpoint by using the console or the AWS CLI\.

**To view Client VPN endpoint routes \(console\)**

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint for which to view routes and choose **Route Table**\.

**To view Client VPN endpoint routes \(AWS CLI\)**  
Use the [describe\-client\-vpn\-routes](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-client-vpn-routes.html) command\.

## Delete an endpoint route<a name="cvpn-working-routes-delete"></a>

You can only delete routes that you added manually\. You can't delete routes that were automatically added when you associated a subnet with the Client VPN endpoint\. To delete routes that were automatically added, you must disassociate the subnet that initiated its creation from the Client VPN endpoint\.

You can delete a route from a Client VPN endpoint by using the console or the AWS CLI\.

**To delete a Client VPN endpoint route \(console\)**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Client VPN Endpoints**\.

1. Select the Client VPN endpoint from which to delete the route and choose **Route Table**\.

1. Select the route to delete, choose **Delete Route**, and choose **Delete Route**\.

**To delete a Client VPN endpoint route \(AWS CLI\)**  
Use the [delete\-client\-vpn\-route](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-client-vpn-route.html) command\.