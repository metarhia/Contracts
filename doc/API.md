# API function contract

- Define API method as:
  - Asynchronous function in async/await syntax or
  - Synchronous function return value or
  - Synchronous function returning `Promise`
- Accepts named arguments in destructuring syntax
`({ arg1, arg2, ...rest }) => {}`
- Optionally use `guard` to validate arguments and results:
`arg1 = guard(arg1, definition)`
- Throw errors or call `reject` in promise

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

## Function with guards

Use guards optionally. You can apply guard for one or a few arguments. You can
apply it multiple times for one argument for complex checking or for combination
of multiple arguments.

```js
async ({ name, city, age }) => {
  name = guard(name, { type: 'string', default: 'Marcus' });
  city = guard(city, { type: 'string', default: 'Roma' });
  age = guard(age, { type: 'number', required: true });

  const result = { name, city, born: age + 121 };

  guard(result.born, { type: 'number', required: true });
  return result;
};
```
