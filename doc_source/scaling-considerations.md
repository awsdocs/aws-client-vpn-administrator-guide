# Client VPN scaling considerations<a name="scaling-considerations"></a>

When you create a Client VPN endpoint, consider the maximum number of concurrent VPN connections that you plan to support\. You should take into account the number of clients that you currently support, and whether your Client VPN endpoint can meet additional demand if needed\. 

The following factors affect the maximum number of concurrent VPN connections that can be supported on a Client VPN endpoint\.

**Client CIDR range size**  
When you [create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create), you must specify a client CIDR range, which is an IPv4 CIDR block between a /12 and /22 netmask\. Each VPN connection to the Client VPN endpoint is assigned a unique IP address from the client CIDR range\. A portion of the addresses in the client CIDR range are also used to support the availability model of the Client VPN endpoint, and cannot be assigned to clients\. You cannot change the client CIDR range after you create the Client VPN endpoint\.  
In general, we recommend that you specify a client CIDR range that contains twice the number of IP addresses \(and therefore concurrent connections\) that you plan to support on the Client VPN endpoint\. 

**Number of associated subnets**  
When you [associate a subnet](cvpn-working-target.md) with a Client VPN endpoint, you enable users to establish VPN sessions to the Client VPN endpoint\. You can associate multiple subnets with a Client VPN endpoint for high availability, and to enable additional connection capacity\.   
The following are the number of supported concurrent VPN connections based on the number of subnet associations for the Client VPN endpoint\.      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/scaling-considerations.html)
You cannot associate multiple subnets from the same Availability Zone with a Client VPN endpoint\. Therefore, the number of subnet associations also depends on the number of Availability Zones that are available in an AWS Region\.

For example, if you expect to support 8,000 VPN connections to your Client VPN endpoint, specify a minimum client CIDR range size of `/18` \(16,384 IP addresses\), and associate at least 2 subnets with the Client VPN endpoint\.

If you’re unsure what the number of expected VPN connections is for your Client VPN endpoint, we recommend that you specify a size `/16` CIDR block or larger\.

For more information about the rules and limitations for working with client CIDR ranges and target networks, see [Limitations and rules of Client VPN](what-is.md#what-is-limitations)\.

For more information about quotas for your Client VPN endpoint, see [AWS Client VPN quotas](limits.md)\.