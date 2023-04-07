# Metacom protocol Version 3

[Metacom](https://github.com/metarhia/metacom) is a top-level application
protocol for RPC (remote procedure call) and large binary objects transfer.
Metacom requires frame-based transport, for example
[websocket](https://tools.ietf.org/html/rfc6455).
[TCP](https://tools.ietf.org/html/rfc793) and
[TLS](https://tools.ietf.org/html/rfc8446) can be used as underlying transport
but additional converting layer (described below) is needed to have frame-based
transport instead of stream-based one. Metacom can support multiple serialization
formats like [JSON](https://tools.ietf.org/html/rfc8259) (by default) and
[MDSF](https://github.com/metarhia/mdsf) (JSON5 implementation),
[V8 serialization API](https://nodejs.org/api/v8.html#v8_serialization_api),
[BSON](http://bsonspec.org/), etc. Metacom is a simplification and modernization
of [JSTP protocol](https://github.com/metarhia/jstp).

## Packet types

There are following packet types: `call`, `callback`, `event`, `stream`, `ping`.

## Calls and callbacks

Call format:
```js
{
  type: "call",
  id: number,      // we need call id to match result (metacom callback packet)
  method: string,  // method name format: unit/name or unit.version/name, example: "chat.5/send"
  args: object,    // we use named arguments to be able marc them optional
  meta: object,    // field for optional passthrough metadata
}
```

Callback format:
```js
{
  type: "callback",
  id: number,      // call id
  result: any,     // any data (object, array or a single value)
  error: { "code": number, "message": string },
  meta: object,    // field for optional passthrough metadata
}
```

Examples:
```json
{"type":"call","id":110,"method":"auth/signIn","args":{"login":"marcus","password":"marcus"}}
{"callback":110,"result":{"token":"2bSpjzG8lTSHaqihGQCgrldypyFAsyme"}}
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
  id: number,
  name: string,
  size: number,
}
```

Stream chunk:
```js
{
  type: "stream",
  dest: 'server' | 'client',
  id: number,
} // Next frame: `Buffer`
```

Stream finalization:
```js
{
  type: "stream",
  dest: 'server' | 'client',
  id: number,
  status: "end",
}
```

Stream termination:
```js
{
  type: "stream",
  dest: 'server' | 'client',
  id: number,
  status: "terminate",
}
```

## Ping packets

Client may periodically generate ping packets `{}` (empty objects, without
fields and id) to test connection. Server should also answer with empty object
`{}`.
