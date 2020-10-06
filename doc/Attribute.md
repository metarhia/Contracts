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
|   `.type`          | `container, link, dictionary, tag, flag, tree`        |
|   `.display`       | `master detail, list, autocomplete, paged`            |
| `order`            | `<number>` attribute order                            |
| `editable`         | `true, false, initial`                                |
| `case`             | `normal, upper, lower, capitalize, title`             |
| `length`           | `{ min: number, max: number }`                        |
| `min`              | `<number>`                                            |
| `max`              | `<number>`                                            |
| `default`          | Value by default                                      |
| `example`          | `<string>` Example value                              |
| `regexp`           | `<string>` Regular expression                         |
| `comments`         | `<string>` comments for developers                    |

| Attribute metadata   | Description                                         |
| -------------------- | --------------------------------------------------- |
| `validate`           |                                                     |
| `format`             |                                                     |
| `validate`           |                                                     |
| `validate`           |                                                     |
