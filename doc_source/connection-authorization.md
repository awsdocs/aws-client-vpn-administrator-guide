# Connection authorization<a name="connection-authorization"></a>

You can configure a *client connect handler* for your Client VPN endpoint\. The handler enables you to run custom logic that authorizes a new connection, based on device, user, and connection attributes\. The client connect handler runs after the Client VPN service has authenticated the device and user\. 

To configure a client connect handler for your Client VPN endpoint, create an AWS Lambda function that takes device, user, and connection attributes as inputs, and returns a decision to the Client VPN service to allow or deny a new connection\. You specify the Lambda function in your Client VPN endpoint\. When devices connect to your Client VPN endpoint, the Client VPN service invokes the Lambda function on your behalf\. Only connections that are authorized by the Lambda function are allowed to connect to the Client VPN endpoint\.

**Note**  
Currently, the only type of client connect handler that is supported is a Lambda function\. 

## Requirements and considerations<a name="client-connect-handler-reqs"></a>

The following are the requirements and considerations for the client connect handler:
+ The name of the Lambda function must begin with the `AWSClientVPN-` prefix\.
+ Lambda function versions are not supported\. When you specify the Lambda function in your Client VPN endpoint, you must specify an [unqualified Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/lambda/latest/dg/configuration-versions.html#versioning-versions-using)\.
+ The Lambda function must be in the same AWS Region and the same AWS account as the Client VPN endpoint\.
+ The Lambda function times out after 30 seconds\. This value cannot be changed\.
+ The Lambda function is invoked synchronously\. It's invoked after device and user authentication, and before the authorization rules are evaluated\.
+ If the Lambda function is invoked for a new connection and the Client VPN service does not get an expected response from the function, the Client VPN service denies the connection request\. For example, this can occur if the Lambda function is throttled, times out, or encounters other unexpected errors, or if the function's response is not in a valid format\.
+ We recommend that you configure [provisioned concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) for the Lambda function to enable it to scale without fluctuations in latency\.
+ If you update your Lambda function, existing connections to the Client VPN endpoint are not affected\. You can terminate the existing connections, and then instruct your clients to establish new connections\. For more information, see [Terminate a client connection](cvpn-working-connections.md#cvpn-working-connections-disassociate)\.
+ If clients use the AWS provided client to connect to the Client VPN endpoint, they must use version 1\.2\.6 or later for Windows, and version 1\.2\.4 or later for macOS\. For more information, see [Connect using the AWS provided client](https://docs.aws.amazon.com/vpn/latest/clientvpn-user/connect-aws-client-vpn-connect.html)\.

## Lambda interface<a name="connection-authorization-lambda"></a>

The Lambda function takes device attributes, user attributes, and connection attributes as inputs from the Client VPN service\. It must then return a decision to the Client VPN service whether to allow or deny the connection\.

**Request schema**  
The Lambda function takes a JSON blob containing the following fields as input\.

```
{
    "connection-id": <connection ID>,
    "endpoint-id": <client VPN endpoint ID>,
    "common-name": <cert-common-name>,
    "username": <user identifier>,
    "platform": <OS platform>,
    "platform-version": <OS version>,
    "public-ip": <public IP address>,
    "client-openvpn-version": <client OpenVPN version>,
    "schema-version": "v1"
}
```
+ `connection-id` — The ID of the client connection to the Client VPN endpoint\.
+ `endpoint-id` — The ID of the Client VPN endpoint\.
+ `common-name` — The device identifier\. In the client certificate that you create for the device, the common name uniquely identifies the device\.
+ `username` — The user identifier, if applicable\. For Active Directory authentication, this is the user name\. For SAML\-based federated authentication, this is `NameID`\. For mutual authentication, this field is empty\.
+ `platform` — The client operating system platform\. 
+ `platform-version` — The version of the operating system\. The Client VPN service provides a value when the `--push-peer-info` directive is present in the OpenVPN client configuration when clients connect to a Client VPN endpoint, and when the client is running the Windows platform\.
+ `public-ip` — The public IP address of the connecting device\.
+ `client-openvpn-version` — The OpenVPN version that the client is using\.
+ `schema-version` — The schema version\. The default is `v1`\.

**Response schema**  
The Lambda function must return the following fields\.

```
{
    "allow": boolean,
    "error-msg-on-failed-posture-compliance": "",
    "posture-compliance-statuses": [],
    "schema-version": "v1"
}
```
+ `allow` — Required\. A boolean \(`true` \| `false`\) that indicates whether to allow or deny the new connection\.
+ `error-msg-on-failed-posture-compliance` — Required\. A string of up to 255 characters that can be used to provide steps and guidance to clients if the connection is denied by the Lambda function\. In the event of failures during the running of the Lambda function \(for example, due to throttling\) the following default message is returned to clients\.

  ```
  Error establishing connection. Please contact your administrator.
  ```
+ `posture-compliance-statuses` — Required\. If you use the Lambda function for [posture assessment](#connection-authorization-posture-assessment), this is a list of statuses for the connecting device\. You define the status names according to your posture assessment categories for devices, for example, `compliant`, `quarantined`, `unknown`, and so on\. Each name can be up to 255 characters in length\. You can specify up to 10 statuses\.
+ `schema-version` — Required\. The schema version\. The default is `v1`\.

You can use the same Lambda function for multiple Client VPN endpoints in the same Region\.

For more information about creating a Lambda function, see [Getting started with AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) in the *AWS Lambda Developer Guide*\.

## Using the client connect handler for posture assessment<a name="connection-authorization-posture-assessment"></a>

You can use the client connect handler to integrate your Client VPN endpoint with your existing device management solution to evaluate the posture compliance of connecting devices\. For the Lambda function to work as a device authorization handler, use [mutual authentication](http://katerini.aka.corp.amazon.com/client-vpn-posture-ug/src/AWSVPNDocs/build/server-root/vpn/latest/clientvpn-admin/client-authentication.html#mutual) for your Client VPN endpoint\. Create a unique client certificate and key for each client \(device\) that will connect to the Client VPN endpoint\. The Lambda function can use the unique common name for the client certificate \(that's passed from the Client VPN service\) to identify the device and fetch its posture compliance status from your device management solution\. You can use mutual authentication combined with user\-based authentication\. 

Alternatively, you can do a basic posture assessment in the Lambda function itself\. For example, you can assess the `platform` and `platform-version` fields that are passed to the Lambda function by the Client VPN service\.

## Enabling the client connect handler<a name="enable-client-connect-handler"></a>

To enable the client connect handler, create or modify a Client VPN endpoint and specify the Amazon Resource Name \(ARN\) of the Lambda function\. For more information, see [Create a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-create) and [Modify a Client VPN endpoint](cvpn-working-endpoints.md#cvpn-working-endpoint-modify)\. 

## Service\-linked role<a name="connection-authorization-slr"></a>

AWS Client VPN automatically creates a service\-linked role in your account called **AWSServiceRoleForClientVPNConnections**\. The role has permissions to invoke the Lambda function when a connection is made to the Client VPN endpoint\. For more information, see [Using service\-linked roles for Client VPN](using-service-linked-roles.md)\.

## Monitoring connection authorization failures<a name="connection-authorization-monitoring"></a>

You can view the connection authorization status of connections to the Client VPN endpoint\. For more information, see [View client connections](cvpn-working-connections.md#cvpn-working-connections-view)\.

When the client connect handler is used for posture assessment, you can also view the posture compliance statuses of devices that connect to your Client VPN endpoint in the connection logs\. For more information, see [Connection logging](connection-logging.md)\. 

If a device fails connection authorization, the `connection-attempt-failure-reason` field in the connection logs returns one of the following failure reasons:
+ `client-connect-failed` — The Lambda function prevented the connection from being established\.
+ `client-connect-handler-timed-out` — The Lambda function timed out\.
+ `client-connect-handler-other-execution-error` — The Lambda function encountered an unexpected error\.
+ `client-connect-handler-throttled` — The Lambda function was throttled\.
+ `client-connect-handler-invalid-response` — The Lambda function returned a response that was not valid\.
+ `client-connect-handler-service-error` — There was a service\-side error during the connection attempt\.