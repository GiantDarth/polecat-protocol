# Errors
Error Objects are similarly defined by the JSON-RPC 2.0 specification to Request and Response objects. In the context of Polecat, they're used by the server to respond to client requests.

## Error Codes
The following codes are defined by the [JSON-RPC 2.0](http://www.jsonrpc.org/specification) spec:
| Error Codes | Message | JSON-RPC 2.0 Meaning |
| ----------- | ------- | -------------------- |
| -32700      | Parse error | Invalid JSON was received by the server. An error occurred on the server while parsing the JSON text. |
| -32600      | Invalid Request | The JSON sent is not a valid Request object.
| -32601      | Method not found | The method does not exist / is not available.
| -32602      | Invalid params | Invalid method parameter(s).
| -32603      | Internal error | Internal JSON-RPC error.
| -32000 to -32099 | Server error | Reserved for the implementation-defined server-errors.

For our use-case, the range of -32000 to -32099 is defined by the Polecat protocol. As of this writing, reserved error -32603 is not required.

Websocket binary frames are to be dropped silently, as the protocol is limited to text-only frames. Instead, binary data SHOULD be encoded as base64 and used directly in the Response Object.

### Polecat Protocol
| Error Codes | Message | Polecat Protocol Meaning |
| ----------- | ------- | ------------------------ |
| -32000      | Not logged in. | Command is not accessible to client while not logged in.
| -32001      | Username taken. | Attempted registration failed as the username is already taken.
| -32002      | Login failed.   | Attempted login failed, e.g. password is incorrect or username doesn't exist.
| -32003      | Client banned. | Either the IP or the registered account is banned from the server.
| -32004      | Already logged in. | User attempted to login while already logged in another account.
| -32005      | Command excessive. | The payload exceeded the message limit based on implementation.
| -32006      | Permission denied. | The client doesn't have the necessary ranking for the command.
| -32007      | Client muted. | Client is muted from the channel.
| -32008      | Max characters met. | The client cannot make anymore characters than the server has set.