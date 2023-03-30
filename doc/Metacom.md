# Metacom protocol

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

```js
// Format:
{"type":"call","id":<Number>,"interface":<String>,"method":<String>,"args":<Object>}
{"type":"callback","id":<Number>,"result":<any>,"error":{"code":<Number>,"message":<String>}}

// Example:
{"type":"call","id":110,"interface":"auth","method":"signIn","args":{"login":"marcus","password":"marcus"}}
{"callback":110,"result":{"token":"2bSpjzG8lTSHaqihGQCgrldypyFAsyme"}}
```

## Events

```js
// Format:
{"type":"event","interface":<String>,"name":<String>,"args":<Object>}

// Example:
{"type":"event","interface":"chat","name":"message","args":{"from":"marcus","message":"Hello!"}}
```

## Streams

```js
// Stream initialization
{"type":"stream","id":<Number>,"name":<String>,"size":<Number>}

// Stream chunk
{"type":"stream","id":<Number>}
// Next frame: <Buffer>

// Stream finalization
{"type":"stream","id":<Number>,"status":"end"}

// Stream termination
{"type":"stream","id":<Number>,"status":"terminate"}
```

## Ping packets

Client may periodically generate ping packets `{}` (empty objects, without
fields and id) to test connection. Server should also answer with empty object
`{}`.
