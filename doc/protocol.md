# Polecat Protocol
The official protocol for Polecat. Works ontop of JSON-RPC, which works ontop of WebSockets.

## Overview
The Polecat protocol is designed with both chat and roleplay server-client systems in mind. While this is a specific use-case for my project's needs, anyone may derive their own protocol so long as they keep the *protocol* itself open and share-alike and keep attribution (specifically Creative Commons Attribution-ShareAlike 4.0 International, see **LICENSE** file.)

The protocol itself is not transport agnostic, but language agnostic. One may write and implement the protcol in any server and/or client so long as they meet the transport standards.

Polecat is a layered protocol, that is built ontop of JSON-RPC 2.0-based messaging, ontop of the WebSockets secure transport protocol.

## Transport
Polecat is built-over WebSockets, following the [RFC-6455](https://tools.ietf.org/html/rfc6455) version. The protocol, to promote always-on encryption and privacy, only specifies using the WebSockets Secure protocol.

## Compatibility
As of this writing (October 2, 2015), the RFC-6455 standard is supported *without plugins* on Internet Explorer 10+, Firefox 23+, Chrome 29+, Safari 6+, Opera 17+, Android Browser 4.4+, and Blackberry Browser 7.0+.

## Messaging
Polecat's messaging system uses [JSON-RPC 2.0](http://www.jsonrpc.org/specification) to transport data to and from the server-client system. All implementations MUST follow the 2.0 version. However, Polecat also extends a specific use-case.
In Polecat, the client might not always need a "meaningful", detailed response, but still needs to know when an error has occured. The Notification object doesn't suit this, as it doesn't allow for any sort of response, even upon errors. Therefore, when a Response object is needed for an error but no meaningful **result* is needed, the protocol MUST set the Response's result to {}, an empty object. When receiving an {}, this is taken to mean that the procedure succeeded.