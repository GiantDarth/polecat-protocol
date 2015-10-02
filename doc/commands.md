# Commands
Polecat is built upon JSON-RPC 2.0. This allows for clients to essentially call certain commands remotely. This may or may not succeed based on the granted permissions of the client.

## Format
### Client
The client will request for calls to be done, sending appropriate arguments on what is needed.
Every message sent to the server must follow the format:

#### Request Object
<dl>
<dt>jsonrpc</dt>
<dd>The version of jsonrpc to be used. EVERY Request must not only contain this, but set to exactly `"2.0"`.
<dt>method</dt>
<dd>The name of the call set forth by the protocol's API. Every call is in lowercase, words separated by _'s.</dd>
<dt>params</dt>
<dd>A JSON structure based on the arguments needed. This MAY be omitted when a parameter is not required. Parameter values may be primitives or JSON structures themselves.</dd>
<dt>id</dt>
<dd>The identifier for the request. (WIP)</dd>
</dl>

*White space is ignored.*
#### Raw Format
`{'jsonrpc': "2.0", 'method': "call_name", 'params': {'param1': 1, 'param2': "example", ...}, 'id': WIP}`
#### Raw Example
`{'jsonrpc': "2.0", 'method': "add_numbers", 'params': {'first_num': 1, 'second_num': 5}, 'id': WIP}`
#### Direction
Server <---- Client

### Server
The server responds to Request objects, either giving a *result* or an *error* if one occured. If the call does not have a return type and the procedure succeeded, the server WILL ALWAYS set *result* to exactly `{}`.

#### Response Object
<dl>
<dt>jsonrpc</dt>
<dd>The version of jsonrpc to be used. EVERY Request must not only contain this, but set to exactly `"2.0"`.
<dt>result</dt>
<dd>The result of a call, which may be a primitive or JSON structure. If an error exists, this MUST NOT exist. If there is no return type but it has succeeded, then the server MUST set this to exactly `{}`.</dd>
<dt>error</dt>
<dd>An error from the processing of the call, as defined in **errors.md**. If a result exists, this MUST NOT exist.</dd>
<dt>id</dt>
<dd>The identifier for the request. It MUST be exactly the same as the appropriate Request Object, otherwise if there was an error, this MUST be set to Null. (WIP)</dd>
</dl>

*White space is ignored.*
#### Raw Format
##### Case 1
`{'jsonrpc': "2.0", 'result': Primitive or JSON obj, 'id': WIP}`
##### Case 2
`{'jsonrpc': "2.0", 'error': Error Object, 'id': WIP}`
##### Case 3
`{'jsonrpc': "2.0", 'error': Error Object, 'id': Null}`
#### Raw Example
##### Case 1
`{'jsonrpc': "2.0", 'result': 5, 'id': WIP}`
##### Case 2
WIP
##### Case 3
WIP
#### Direction
Server ----> Client

#### Notification Object
Think of Polecat's interpretation of Notification objcts as "broadcasts" to the world, aka. the list of clients. Notifications are used for state updates and changes, such as broadcasting an OOC message to clients, or letting other clients know that one client changed the icon for a character.

Notification objects in JSON-RPC 2.0 are like Request objects, only the ID is required to be ommitted. This is the main means of a Polecat server to be the initiator as opposed to the client's Request objects. By requirement, nothing should be sent back, the client shouldn't respond, not even on an error. When an error occurs, it is expected to espond silently (likely from a client disconnect, in which case the client will be removed from the server anyways.)

<dl>
<dt>jsonrpc</dt>
<dd>The version of jsonrpc to be used. EVERY Request must not only contain this, but set to exactly `"2.0"`.
<dt>method</dt>
<dd>The name of the call set forth by the protocol's API. Every call is in lowercase, words separated by _'s.</dd>
<dt>params</dt>
<dd>A JSON structure based on the arguments needed. This MAY be omitted when a parameter is not required. Parameter values may be primitives or JSON structures themselves.</dd>
</dl>

*White space is ignored.*
#### Raw Format
`{'jsonrpc': "2.0", 'method': "call_name", 'params': {'param1': 1, 'param2': "example", ...}}`
#### Raw Example
`{'jsonrpc': "2.0", 'method': "ooc", 'params': {'username': "My Name", 'message': "I'm speaking now.", 'channel': 0, subchannel: 0}}`
#### Direction
Server ----> Client(s)