# Split\-tunnel on AWS Client VPN endpoints<a name="split-tunnel-vpn"></a>

By default, when you have a Client VPN endpoint, all traffic from clients is routed over the Client VPN tunnel\. When you enable split\-tunnel on the Client VPN endpoint, we push the routes on the [Client VPN endpoint route table](cvpn-working-routes.md) to the device that is connected to the Client VPN endpoint\. This ensures that only traffic with a destination to the network matching a route from the Client VPN endpoint route table is routed over the Client VPN tunnel\. 

You can use a split\-tunnel Client VPN endpoint when you do not want all user traffic to route through the Client VPN endpoint\. 

In the following example, split\-tunnel is enabled on the Client VPN endpoint\. Only traffic that's destined for the VPC \(`172.31.0.0/16`\) is routed over the Client VPN tunnel\. Traffic that's destined for on\-premises resources is not routed over the Client VPN tunnel\.

![\[Split tunnel Client VPN endpoint\]](http://docs.aws.amazon.com/vpn/latest/clientvpn-admin/images/client-vpn-split-tunnel.png)

## Split\-tunnel benefits<a name="split-tunnel-benefits"></a>

Split\-tunnel on Client VPN endpoints offers the following benefits:
+ You can optimize the routing of traffic from clients by having only the AWS destined traffic traverse the VPN tunnel\.
+ You can reduce the volume of outgoing traffic from AWS, therefore reducing the data transfer cost\.

## Routing considerations<a name="split-tunnel-routing"></a>

When you enable split\-tunnel on a Client VPN endpoint, all of the routes that are in the Client VPN route tables are added to the client route table when the VPN is established\. This operation is different from the default Client VPN endpoint operation, which overwrites the client route table with the entry 0\.0\.0\.0/0 to route all traffic over the VPN\.

## Enabling\-split\-tunnel<a name="split-tunnel-enable"></a>

You can enable split\-tunnel on a new or existing Client VPN endpoint\. For more information, see the following topics:
+ [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create)
+ [Modify a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-modify)