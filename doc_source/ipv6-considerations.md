# IPv6 Considerations<a name="ipv6-considerations"></a>

Currently the Client VPN service does not support routing IPv6 traffic through the VPN tunnel\. However, there are cases when IPv6 traffic should be routed into the VPN tunnel to prevent IPv6 leak\. IPv6 leak can happen when both IPv4 and IPv6 are enabled and connected to the VPN, but the VPN doesn’t route IPv6 traffic into its tunnel\. In this case, when connecting to an IPv6 enabled destination, you are actually still connecting with your IPv6 address provided by your ISP\. This will leak your real IPv6 address\. The instructions below explain how to route IPv6 traffic into the VPN tunnel\.

The following IPv6\-related directives should be added to your Client VPN configuration file to prevent IPv6 leak:

```
ifconfig-ipv6 arg0 arg1
route-ipv6 arg0
```

An example might be:

```
ifconfig-ipv6 fd15:53b6:dead::2 fd15:53b6:dead::1
route-ipv6 2000::/4
```

In this example, `ifconfig-ipv6 fd15:53b6:dead::2 fd15:53b6:dead::1` will set the local tunnel device IPv6 address to be `fd15:53b6:dead::2` and the remote VPN endpoint IPv6 address to be `fd15:53b6:dead::1`\. 

The next command, `route-ipv6 2000::/4` will route IPv6 addresses from `2000:0000:0000:0000:0000:0000:0000:0000` to `2fff:ffff:ffff:ffff:ffff:ffff:ffff:ffff` into the VPN connection\.

**Note**  
For “TAP” device routing in Windows for example, the second parameter of `ifconfig-ipv6` will be used as route target for `--route-ipv6`\.

Organizations should configure the two parameters of `ifconfig-ipv6` themselves, and can use addresses in `100::/64` \(from `0100:0000:0000:0000:0000:0000:0000:0000` to `0100:0000:0000:0000:ffff:ffff:ffff:ffff`\) or `fc00::/7` \(from `fc00:0000:0000:0000:0000:0000:0000:0000` to `fdff:ffff:ffff:ffff:ffff:ffff:ffff:ffff`\)\. `100::/64` is Discard\-Only Address Block, and `fc00::/7` is Unique\-Local\.

Another example:

```
ifconfig-ipv6 fd15:53b6:dead::2 fd15:53b6:dead::1
route-ipv6 2000::/3
route-ipv6 fc00::/7
```

In this example, the configuration will route all currently allocated IPv6 traffic into the VPN connection\.

**Verification**  
Your organization will likely have its own tests\. A basic verification is to set up a full tunnel VPN connection, then run ping6 to an IPv6 server using the IPv6 address\. The IPv6 address of the server should be in the range specified by the `route-ipv6` command\. This ping test should fail\. However, this may change if IPv6 support is added to the Client VPN service in the future\. If the ping is successful and you are able to access public sites when connected in full tunnel mode, you may need to do further troubleshooting\. You can also test by using some publicly available tools like [ipleak\.org](https://ipleak.org/) as well\.