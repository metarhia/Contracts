# API function contract

- Define API method as:
  - Asynchronous function in async/await syntax or
  - Synchronous function returning `Promise`
- Accepts named arguments in destructuring syntax:
`({ arg1, arg2, ...rest }) => {}`
- Throw errors or call `reject` in promise.
- Extended function declaration includes optional elements: parameters and
results schema, imperative parameter and result validation functions, execution
timeout.
- Functions may access other modules with `api` and `application` namespaces,
injected to sandbox and frozen.
- Global context in sandboxes is frozen to prevent global polution.
- Global context has no recursion (like `sandbox.global = sandbox`) so domain and
api code can't access global context by identifier `global`.
- Other common namespaces (injected to sandbox):
  - `console` - same interface as native `console` but different implementation;
  - `setTimeout`, `setImmediate`, `setInterval`, `clearTimeout`,
  `clearImmediate`, `clearInterval` are available from sandbox global;
- Other common namespaces restricted in sandbox:
  - `require` - is restricted in sandbox (not accessible);
  - `global` - is restricted in sandbox;
  - `module` and `exports` are restricted in sandbox;

## API methods different contracts

Simple example:
```js
async ({ a, b }) => a + b;
```

Promise-returning function:
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

Asynchronous function in async/await syntax:
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
- `caption: string` (optional) - method display name;
- `description: string` (optional) - method description;
- `access: string` (optional, default: `logged`) - method access definition;
- `parameters: Schema` (optional) - parameters declarative schema;
- `validate: function` (optional) - parameters imperative validation function;
- `timeout: number` (optional) - execution timeout in milliseconds;
- `queue: { concurrency: number, size: number, timeout: number }` (optional) -
  maximum number of concurrent requests, queue size, and timeout;
- `sirializer: string` (optional, default: `json`) - serialization format;
- `protocols: Array<string>` (optional, default: server default) - set `['http']`
  to allow web hooks, (allowed values: `http`, `https`, `ws`, `wss`);
- `deprecated: boolean` (optional) - deprecated methods marked with `true`;
- `method: function` - async/await or promise-returning function;
- `returns: Schema` (optional) - returning value declarative schema;
- `tests: function` (test) - returning imperative test;
- `cases: Array<TestCase>` - declarative tests (see ecample);
- `examples: Array<object>` (optional) - array of examples;
- `example: object` (optional) - single example (shorthand);
- `errors: Array<object>` (optional) - collection of possible error;

Example method with a contract:
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

  method: async ({ a, b }) => {
    const result = a + b;
    return result;
  },

  returns: { type: 'number' },

  tests: (test) => {
    const res = this.method({ a: 3, b: 7 });
    test.strictSame(res, 10);
    test.end();
  },

  cases: [
    [{ a: 3, b: 7 }, 10],
    [{ a: 10, b: 20 }, (result) => retult === 30],
  ],
});
```

Example method with timeout and queue:
```js
({
  timeout: 1000,

  queue: {
    concurrency: 1,
    size: 200,
    timeout: 3000,
  },

  method: async ({ a, b }) => {
    const result = a + b;
    return result;
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

## Method access definition

Possible values:
- `public` - authentication isn't required;
- `session` - allowed clients with started session (anonymous as well);
- `logged` - any authenticated user allowed;
- `{ group: 'name' }` - only group members allowed;
- `{ login: 'name' }` - only certain user allowed;

```js
({
  access: 'public',

  method: async ({ person, address }) => {
    const addressId = await api.gs.create(address);
    person.address = addressId;
    const personId = await api.gs.create(person);
    return personId;
  },
});
```
