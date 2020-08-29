# Error-handling guidelines

There is a difference between error and exception. Error is a normal result and
regular application behaviour that should not break execution sequence while
exception breaks execution sequence (serving client-side request or certain
asynchronous business-logic scenario).

## Return error from method

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
`{"callback":1,"result":{"key":"value"}}`

Result with error (metacom packet):
`{"callback":1,"error":{"message":"Data is not valid","code":10}}`

## Rise exception

- Application server will return HTTP 500 or abstract RPC error code.
- Application server may transfer exceptions over the network without stack
trace and sensitive error messages (all this data will be logged instead).

```js
(async ({ ...args }) => {
  throw new Error('Method is not implemented');
});
```

Result with exception (metacom packet):<br/>
`{"callback":1,"error":{"message":"Internal Server Error","code":500}}`

How to override error codes:<br/>
`throw new Error('Method is not implemented', 404);`<br/>
This will take error message from code:<br/>
`{"callback":1,"error":{"message":"Not found","code":404}}`

If you specify unknown code like this:<br/>
`throw new Error('Method is not implemented', 12345);`<br/>
This will generate: `"Internal Server Error"` with `"code":500`
