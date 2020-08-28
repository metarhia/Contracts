# Error-handling guidelines

There is a difference between error and exception. Error is a normal result and
regular application behaviour that should not break execution sequence while
exception breaks execution sequence (serving client-side request or certain
asynchronous business-logic scenario).

## Rerurn error from method

```js
(async ({ ...args }) => {
  const data = await doSomething();
  if (domain.module.validate(data)) {
    return data;
  } else {
    return new Error('Data is not valid', 10);
  }
});
```

Result with data (metacom packet):
```js
{"callback":1,"result":{"key":"value"}}
```

Result with error (metacom packet):
```js
{"callback":1,"error":{"message":"Data is not valid","code":10}}
```

## Rise exception

- Application server will return HTTP 500 or abstract RPC error code.
- Application server may tranfer exception over network without stack trace
and sensitive error message (all this data will be logged instead).

```js
(async ({ ...args }) => {
   throw new Error('Method is not implemented', 20);
});
```

Result with exception (metacom packet):
```js
{"callback":1,"error":{"message":"Internal Server Error","code":500}}
```
