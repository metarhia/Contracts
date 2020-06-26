# Metacom protocol

Metacom is a top-level application protocol for RPC (remote procedure call) and
large binary objects transfer. Metacom requires frame-based transport, for
example websocket. TCP and TLS can be used as underlying transport but additional
converting layer (described below) is needed to have frame-based transport
instead of stream-based one. Metacom can support multiple serialization formats
like JSON (by default) and MDSF (JSON5 implementation), V8 serialization API,
BSON, etc. Metacom is a simplification and modernization of JSTP protocol.

## Packet types

There are following packet types: `call`, `callback`, `event`, `stream`.

## Calls and callbacks

```js
// Format:
{"call":<Number>,"interface.version/method":{<arguments>}}
{"callback":<Number>,"result":<any>,"error":{"code":<Number>,"message":<String>}}

// Example:
{"call":110,"auth/signIn":{"login":"marcus","password":"marcus"}}
{"callback":110,"result":{"token":"2bSpjzG8lTSHaqihGQCgrldypyFAsyme"}}
```

## Events

```js
// Format:
{"event":<Number>,"interface/event":{<arguments>}}

// Example:
{"event":-25,"chat/message":{"from":"marcus",message:"Hello!"}}
```

## Streams

```js
{"stream":<Number>,"chunk":<Number>"}<Buffer>
```
