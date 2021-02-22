# Attribute/value schema

| Attribute metadata | Description                                               |
| ------------------ | --------------------------------------------------------- |
| `type`             | Data type or name of reference Entity                     |
| `get`              | Attribute getter: `get() {}`                              |
| `set`              | Attribute setter: `set(value) {}`                         |
| `required`         | `boolean`, default: `true`                                |
| `nullable`         | `boolean`, default: `false` for indexes, `true` for other |
| `index`            | `boolean`, default: `false`                               |
| `unique`           | `boolean`, default: `false`                               |
| `delete`           | `cascade, restrict, no action, set null, set default`     |
| `update`           | `cascade, restrict, no action, set null, set default`     |
| `control`          | `string`, UI control name                                 |
| `lookup`           | `{ caption, key, result, type }`                          |
| `enum`             | `array` possible values                                   |
| `flag`             | `array` possible values                                   |
| `lookup.type`      | `container, link, dictionary, tag, flag, tree`            |
| `lookup.display`   | `master detail, list, autocomplete, paged`                |
| `order`            | `number`, attribute order                                 |
| `editable`         | `boolean`, editable in UI                                 |
| `case`             | `normal, upper, lower, capitalize, title`                 |
| `length`           | `number` or `{ min: 'number', max: 'number' }`            |
| `min`              | `number`                                                  |
| `max`              | `number`                                                  |
| `default`          | Value by default                                          |
| `example`          | `string`, example value                                   |
| `hint`             | `string`, user hint for UI                                |
| `pattern`          | `string`, regular expression                              |
| `comments`         | `string`, comments for developers                         |
| `include`          | `string`, entity name to be included here                 |

| Attribute metadata   | Description                                         |
| -------------------- | --------------------------------------------------- |
| `validate`           | `(record, previous) => result: boolean`             |
| `format`             | `value: <type> => result: <type>`                   |
| `parse`              | `string => <type>`                                  |
| `serialize`          | `<type> => <string>`                                |
