# Application configuration contract

- Config files are JavaScript files containing object expression `({ });`
- Don't use `'use strict';` in config files, configs are always in strict mode
- Configs are placed in `application/config`
- We may have different versions for each config file to be loaded depending on
server environment mode: `SERVER_MODE` variable. For example: `config/log.js`
and `config/log.test.js`. First file will be loaded by default, second one will
be loaded only if `SERVER_MODE=test`

Example:
```js
({
  keepDays: 100, // Delete files after N days
  writeInterval: Duration('3s'), // Flush log to disk interval
  writeBuffer: 64 * 1024, // Buffer size 64kb
  toStdout: ['system', 'fatal', 'error']
});
```
