# Task definition file

- Task files are JavaScript files containing object expression `({ });`
- Don't use `'use strict';` in task files, tasks are always in strict mode
- Tasks are placed in `application/tasks`
- Task fields:
  - `caption` - task display name
  - `execute` - execution type, allowed values: `single`, `server`, `instance`
  - `time` (optional) - defines certain time in UTC
  - `interval` (optional) - defines interval in milliseconds
  - `timeout` (optional) - task execution timeout in milliseconds
  - `run` - async function to execute
- Task properties in runtime:
  - `name` - file name without extension used to access tasks
  - `success` - last execution status: `undefined`, `true`, `false`
  - `error` - last error or `null` (if success)
  - `lastStart` - last execution start time or `null` (never)
  - `lastEnd` - last execution end time or `null` (never)
  - `executing` - current status: `true`, `false`
  - `active` - active in current process: `true`, `false`
  - `count` - executing count `number`

Example:
```js
({
  caption: 'Task One',
  execute: 'server', // execute once per server

  // You can use either time or interval
  // time: '12:30', // run at certain time
  interval: '2s', // interval between task executions

  timeout: 1000, // task execution timeout in milliseconds

  run: async task => {
    console.log('Execute task one');
    console.dir({ task });
    // throw on fail
  },
});
```
