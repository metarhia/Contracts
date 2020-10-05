# Domain and library module contract

|                      |                                               |
| -------------------- | --------------------------------------------- |
| `application/domain` | Domain code (use cases or business-logic)     |
| `application/lib`    | Libraries, helpers, adapters for dependencies |
|                      |                                               |

Place your methods in file structure like in following example:
`/application/domain/submodule/functionName.js` and loader will read all `.js`
files linking them into namespaces like this `domain.submodule.functionName`.
