# Connection logging<a name="connection-logging"></a>

Connection logging is a feature of AWS Client VPN that enables you to capture *connection logs* for your Client VPN endpoint\. 

A connection log contains *connection log entries*\. Each connection log entry contains information about a connection event, which is when a client \(end user\) connects, attempts to connect, or disconnects from your Client VPN endpoint\. You can use this information to run forensics, analyze how your Client VPN endpoint is being used, or debug connection issues\.

Connection logging is available in all Regions where AWS Client VPN is available\. Connection logs are published to a CloudWatch Logs log group in your account\.

**Note**  
Failed mutual authentication attempts are not logged\.

## Connection log entries<a name="connection-log-entries"></a>

A connection log entry is a JSON\-formatted blob of key\-value pairs\. The following is an example connection log entry\.

```
{
    "connection-log-type": "connection-attempt",
    "connection-attempt-status": "successful",
    "connection-reset-status": "NA",
    "connection-attempt-failure-reason": "NA",
    "connection-id": "cvpn-connection-abc123abc123abc12",
    "client-vpn-endpoint-id": "cvpn-endpoint-aaa111bbb222ccc33",
    "transport-protocol": "udp",
    "connection-start-time": "2020-03-26 20:37:15",
    "connection-last-update-time": "2020-03-26 20:37:15",
    "client-ip": "10.0.1.2",
    "common-name": "client1",
    "device-type": "mac",
    "device-ip": "98.247.202.82",
    "port": "50096",
    "ingress-bytes": "0",
    "egress-bytes": "0",
    "ingress-packets": "0",
    "egress-packets": "0",
    "connection-end-time": "NA",
    "username": "joe"
    }
```

A connection log entry contains the following keys:
+ `connection-log-type` — The type of connection log entry \(`connection-attempt` or `connection-reset`\)\.
+ `connection-attempt-status` — The status of the connection request \(`successful`, `failed`, `waiting-for-assertion`, or `NA`\)\.
+ `connection-reset-status` — The status of a connection reset event \(`NA` or `assertion-received`\)\.
+ `connection-attempt-failure-reason` — The reason for the connection failure, if applicable\.
+ `connection-id` — The ID of the connection\.
+ `client-vpn-endpoint-id` — The ID of the Client VPN endpoint to which the connection was made\.
+ `transport-protocol` — The transport protocol that was used for the connection\.
+ `connection-start-time` — The start time of the connection\.
+ `connection-last-update-time` — The last update time of the connection\. This value is periodically updated in the logs\.
+ `client-ip` — The IP address of the client, which is allocated from the client IPv4 CIDR range for the Client VPN endpoint\.
+ `common-name` — The common name of the certificate that's used for certificate\-based authentication\.
+ `device-type` — The type of device used for the connection by the end user\.
+ `device-ip` — The public IP address of the device\.
+ `port` — The port number for the connection\.
+ `ingress-bytes` — The number of ingress \(inbound\) bytes for the connection\. This value is periodically updated in the logs\.
+ `egress-bytes` — The number of egress \(outbound\) bytes for the connection\. This value is periodically updated in the logs\.
+ `ingress-packets` — The number of ingress \(inbound\) packets for the connection\. This value is periodically updated in the logs\.
+ `egress-packets` — The number of egress \(outbound\) packets for the connection\. This value is periodically updated in the logs\.
+ `connection-end-time` — The end time of the connection\. The value is `NA` if the connection is still in progress or if the connection attempt failed\.
+ `posture-compliance-statuses` — The posture compliance statuses returned by the [client connect handler](connection-authorization.md), if applicable\.
+ `username` — The username is recorded when user\-based authentication \(AD or SAML\) is used for the endpoint\.
+ `connection-duration-seconds` — The duration of a connection in seconds\. Equal to the difference between "connection\-start\-time" and "connection\-end\-time"\.

For more information about enabling connection logging, see [Working with connection logs](cvpn-working-with-connection-logs.md)\.