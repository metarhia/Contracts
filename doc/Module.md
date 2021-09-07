# Domain and library module contract

| Directory            | Description                                   |
| -------------------- | --------------------------------------------- |
| `application/domain` | Domain code (use cases or business-logic)     |
| `application/lib`    | Libraries, helpers, adapters for dependencies |

Place your methods in file structure like in following example:
`/application/domain/submodule/functionName.js` and loader will read all `.js`
files linking them into namespaces like this `domain.submodule.functionName`.

## Single method module

Following file `/application/domain/prepareReport.js` with content:
```js
(async (title, where) => {
  const records = await domain.db.select('Report', ['*'], where);
  const data = await domain.reports.prepare(records);
  const result = await lib.pdf.render(title, data);
  return result;
});
```
will export a single method accessible as `domain.prepareReport`.

## Module with multiple methods, constants and private fields

Following file `/application/lib/moduleName.js` with content:
```js
({
  CONST_NAME: 1000,

  fieldName: { data: 'value' },

  syncMethodName(args) {
    const result = { data: this.privateField };
    return result;
  }

  async asyncMethodName(args) {
    const result = { data: 'method body' };
    return result;
  }
});
```
describes module and will generate namespace `lib.moduleName` where constant, fields,
and methods will be accessible as `lib.moduleName.CONST_NAME`, `lib.moduleName.fieldName`,
`lib.moduleName.syncMethodName`, and `lib.moduleName.asyncMethodName`.
