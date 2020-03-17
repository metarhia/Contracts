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
- `caption` - method display name
- `description` - method description
- `parameters` (optional) - parameters declarative schema
- `validate` (optional) - parameters imperative validation function
- `timeout` (optional) - execution timeout in milliseconds
- `method` - sync or async lambda implementing method
- `returns` (optional) - returning value declarative schema
- `assert` (optional) - returning value imperative assertion function
- `examples` - array of examples
- `example` - single example (shorthand)
- `errors` - collection of possible error

```js
({
  parameters: {
    a: { type: 'number' },
    b: { type: 'number' },
    // or shorthand `a: 'number'`
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

## External schemas

```js
({
  parameters: {
    person: { domain: 'Person' },
    address: { domain: 'Address' },
  },

  method: async ({ person, address }) => {
    const addressId = await api.gs.create(address);
    person.address = addressId;
    const personId = await api.gs.create(person);
    return personId;
  },

  returns: { type: 'number' },
});
```

## Possible errors

```js
({
  parameters: {
    person: { domain: 'Person' },
    address: { domain: 'Address' },
  },

  method: async ({ person, address }) => {
    const addressId = await api.gs.create(address);
    person.address = addressId;
    const personId = await api.gs.create(person);
    return personId;
  },

  returns: { type: 'number' },

  errors: {
    contract: 'Invalid arguments',
    fail: 'Person and address can not be saved to database',
    result: 'Invalid result',
  },
});
```

## Invocation example

```js
({
  example: {
    parameters: {
      person: { name: 'Marcus', surname: 'Aurelius', born: 121 },
      address: { country: 'Pax Romana', city: 'Roma' },
    },
    returns: { id: 10792532194309 },
    // or throws: 'Error message',
  },

  method: async ({ person, address }) => {
    const addressId = await api.gs.create(address);
    person.address = addressId;
    const personId = await api.gs.create(person);
    return personId;
  },
});
```

## Multiple invocation examples

```js
({
  examples: [
    {
      parameters: {
        person: { name: 'Marcus', surname: 'Aurelius', born: 121 },
        address: { country: 'Pax Romana', city: 'Roma' },
      },
      returns: { id: 10792532194309 },
    },
    {
      parameters: {
        person: { name: 'Antoninus', surname: 'Pius', born: 138 },
        address: { country: 'Pax Romana', city: 'Roma' },
      },
      returns: { id: 10792532194308 },
    },
  ],

  method: async ({ person, address }) => {
    const addressId = await api.gs.create(address);
    person.address = addressId;
    const personId = await api.gs.create(person);
    return personId;
  },
});
```

## Caption and description

```js
({
  caption: 'Create person with address',

  description: 'Store person and address to database with relation',

  method: async ({ person, address }) => {
    const addressId = await api.gs.create(address);
    person.address = addressId;
    const personId = await api.gs.create(person);
    return personId;
  },
});
```
