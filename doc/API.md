# API function contract

- Define `AcyncFunction` in async/await syntax or return `Promise` from synchronous `Function`
- Acceps named arguments in destructuring syntax `({ arg1, arg2, ...rest }) => {}`
- Use `guard` to validate arguments and results: `arg1 = guard(arg1, definition)`
- Throw errors in `AcyncFunction` or `reject` promise

## Simple function

```js
async ({ a, b }) => a + b;
```

## Promise returning synchronous function

```js
({ a, b }) => Promise.resolve(a + b);
```

## Function with guards

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
