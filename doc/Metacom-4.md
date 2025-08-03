# Metacom protocol Version 4

[Metacom](https://github.com/metarhia/metacom) is a top-level application
protocol for RPC (remote procedure call) and large binary objects transfer.
Metacom requires frame-based of stream-based transport, for example
[websocket](https://tools.ietf.org/html/rfc6455).
[TCP](https://tools.ietf.org/html/rfc793) and
[TLS](https://tools.ietf.org/html/rfc8446) can be used as underlying transport
but additional converting layer (described below) is needed to have frame-based
transport on the top of stream-based one. Metacom can support multiple serialization
formats like [JSON](https://tools.ietf.org/html/rfc8259) (by default) and
[MDSF](https://github.com/metarhia/mdsf) (JSON5 implementation),
[V8 serialization API](https://nodejs.org/api/v8.html#v8_serialization_api),
[BSON](http://bsonspec.org/), etc. Metacom is a simplification and modernization
of [JSTP protocol](https://github.com/metarhia/jstp).

## Packet types

There are following packet types: `call`, `callback`, `event`, `stream`, `ping`.

## Calls and callbacks

Call ID is a string, use for example UUIDv4 or UUID64 (UUIDv4 converted to BASE64).

Call format:
```js
{
  type: "call",
  id: string,      // unique call id to match result (metacom callback packet)
  method: string,  // method name format: unit/name or unit.version/name, example: "chat.5/send"
  args: object,    // we use named arguments to be able mark them optional
  meta: object,    // field for optional passthrough metadata
}
```

Callback format:
```js
{
  type: "callback",
  id: string,       // unique call id
  result: unknown,  // any data (object, array or a single value)
  error: { "code": number, "message": string },
  meta: object,    // field for optional passthrough metadata
}
```

Examples:
```json
{"type":"call","id":"VrMsUTWIRNiAxJXqpe2ghg","method":"auth/signIn","args":{"login":"marcus","password":"marcus"}}
{"type":"callback","id":"VrMsUTWIRNiAxJXqpe2ghg","result":{"token":"2bSpjzG8lTSHaqihGQCgrldypyFAsyme"}}
```

## Events

Format:
```js
{
  type: "event", // events have no packet id, unlike call packets and events in metacom version 2
  name: string,  // event name in format: unit/name, example: "chat/message"
  data: object,  // attached data
  meta: object,  // field for optional passthrough metadata
}
```

Example:
```json
{"type":"event","name":"unit/message","data":{"from":"marcus","message":"Hello!"}}
```

## Streams

Stream initialization:
```js
{
  type: "stream",
  dest: 'server' | 'client',
  id: string,
  name: string,
  size: number,
}
```

Stream chunk:
```js
{
  type: "stream",
  dest: 'server' | 'client',
  id: string,
} // Next frame: `Buffer`
```

Stream finalization:
```js
{
  type: "stream",
  dest: 'server' | 'client',
  id: string,
  status: "end",
}
```

Stream termination:
```js
{
  type: "stream",
  dest: 'server' | 'client',
  id: string,
  status: "terminate",
}
```

## Ping packets

Client may periodically generate ping packets `{}` (empty objects, without
fields and id) to test connection. Server should also answer with empty object
`{}`.
