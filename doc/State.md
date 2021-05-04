# State definition

## State scope

- `session` - value unique per session
- `user` - value unique per user
- `thread` - value unique per thread
- `process` - value unique per process
- `global` - globally unique value

## State synchronization type

- `counter` - number type with increment / decrement operations
- `set` - Grow-only Set
- `exclusive` - value protected with exclusive lock
- `mrsw` - value protected with multiple-read and single-write
- `optimistic` - not protected with locking
- `cas` - protected with Compare-and-swap operations

## Example

File: `application/state/location.js`
```js
({
  location: {
    latitude: 'number',
    longitude: 'number',
  }

  initial: {
    latitude: 48.864716,
    longitude: 2.349014,
  },

  state: 'cas',
  scope: 'user',
});
```

File: `application/state/reports.js`
```js
({
  reports: 'number',
  initial: 0,
  state: 'counter',
  scope: 'global',
});
```

File: `application/state/participants.js`
```js
({
  participants: 'Set',
  initial: new Set(),
  state: 'set',
  scope: 'thread',
});
```

File: `application/api/location.1/report.js`
```js
async reportLocation({ latitude, longitude }) => {
  context.reports++;
  context.location = { latitude, longitude };
  context.participants.add(context.client.id);
};
