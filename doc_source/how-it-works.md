# How AWS Client VPN Works<a name="how-it-works"></a>

With AWS Client VPN, there are two types of user personas that interact with the Client VPN endpoint: administrators and clients

The *administrator* is responsible for setting up and configuring the service\. This involves creating the Client VPN endpoint, associating the target network, and configuring the authorization rules, and setting up additional routes \(if required\)\. After the Client VPN endpoint is set up and configured, the administrator downloads the Client VPN endpoint configuration file and distributes it to the clients who need access\. The Client VPN endpoint configuration file includes the DNS name of the Client VPN endpoint and certificate information required to establish a VPN session\. For more information about setting up the service, see [Getting Started with Client VPN](cvpn-getting-started.md)\.

The *client* is the end\-user\. This is the person who connects to the Client VPN endpoint to establish a VPN session\. The client establishes the VPN session from their local computer or mobile device using an OpenVPN\-based VPN client application\. After they have established the VPN session, they can securely access the resources in the VPC in which the associated subnet is located\. They can also access other resources in AWS or an on\-premises network if the required route and authorization rules have been configured\. For more information about connecting to a Client VPN endpoint to establish a VPN session, see [Getting Started](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/user-getting-started.html) in the *AWS Client VPN User Guide*\.

The following graphic illustrates the basic Client VPN architecture\.

![\[Client VPN architecture\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/architecture.png)