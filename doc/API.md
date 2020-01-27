# API function contract

- Define API method as:
  - Asynchronous function in async/await syntax or
  - Synchronous function return value or
  - Synchronous function returning `Promise`
- Accepts named arguments in destructuring syntax
`({ arg1, arg2, ...rest }) => {}`
- Throw errors or call `reject` in promise
- Extended function declatation includes optional elements: parameters and
results schema, imperative parameter and result validation functions, execution
timeout.

## API methods different contracts

Pure synchronous function. Note: it's blocking, so you can use it just for
retrieving data from memory structures with simple pre/post calculations.

```js
({ a, b }) => a + b;
```

```js
({ name, delta }) => application.counters[name] += delta;
```

Promise-returning synchronous function with revealing constructor
```js
({ id, object }) => new Promise((resolve, reject) => {
  readObject(id, (err, record) => {
    if (err) return reject(err);
    const obj = Object.assign(record, object);
    updateObject(id, obj, (err, success) => {
      if (err) return reject(err);
      resolve(success);
    });
  });
});
```

Asynchronous function in async/await syntax
```js
async ({ id, object }) => {
  const record = await readObject(id);
  const obj = Object.assign(record, object);
  const success = await updateObject(id, obj);
  return success;
});
```

## Extended function declaration

Declaration elements (fields):
- `parameters` (optional) parameters declarative schema
- `validate` (optional) parameters imperative validation function
- `timeout` (optional) execution timeout in milliseconds
- `method` sync or async lambda implementing method
- `returns` (optional) returning value declarative schema
- `assert` (optional) returning value imperative assertion function

```js
({
  parameters: {
    a: { type: 'number' },
    b: { type: 'number' },
  },

  validate: ({ a, b }) => {
    if (a % 3 === 0) throw new Error('Expected `a` to be multiple of 3');
    if (b % 5 === 0) throw new Error('Expected `b` to be multiple of 5');
  },

  timeout: 1000,

  method: async ({ a, b }) => {
    const result = a + b;
    return result;
  },

  returns: { type: 'number' },

  assert: ({ a, b }, result) => {
    if (result < a && result < b) {
      throw new Error('Result expected to be greater than parameters');
    }
  },
});
```
