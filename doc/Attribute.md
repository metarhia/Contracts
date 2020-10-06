# Attribute/value schema

| Attribute metadata | Description                                           |
| ------------------ | ----------------------------------------------------- |
| `type`             | Data type or name of reference Entity                 |
| `required`         | `false`, default: `true`                              |
| `nullable`         | default: `false` for indexed fields, `true` for other |
| `index`            | `true`, default: `false`                              |
| `unique`           | `true`, default: `false`                              |
| `delete`           | `cascade, restrict, no action, set null, set default` |
| `update`           | `cascade, restrict, no action, set null, set default` |
| `control`          | UI control name                                       |
| `lookup`           | `{ caption, key, result, type }`                      |
| `lookup.type`      | `container, link, dictionary, tag, flag, tree`        |
| `lookup.display`   | `master detail, list, autocomplete, paged`            |
| `order`            | `<number>` attribute order                            |
| `editable`         | `true, false, initial`                                |
| `case`             | `normal, upper, lower, capitalize, title`             |
| `length`           | `{ min: number, max: number }`                        |
| `min`              | `<number>`                                            |
| `max`              | `<number>`                                            |
| `default`          | Value by default                                      |
| `example`          | `<string>` Example value                              |
| `hint`             | `<string>` User hint for UI                           |
| `regexp`           | `<string>` Regular expression                         |
| `comments`         | `<string>` Comments for developers                    |
| `include`          | `<string>` Entity name to be included here            |

| Attribute metadata   | Description                                         |
| -------------------- | --------------------------------------------------- |
| `validate`           |                                                     |
| `format`             |                                                     |
| `validate`           |                                                     |
