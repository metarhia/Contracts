# Metaschema: data structure schemas

## Metaschema core types

- Primitive types: `number`, `bigint`, `string`, `boolean`, `symbol`
- Structural types: `object`, `function`

## Domain model types

- Integer number: `int`
- Money: `money`
- Date: `date`, `time`, `datetime`
- Geometry: `point`, `path`, `polygon`
- BLODs: `text`, `buffer`

## Machine-oriented types

- Signed integers: `i8`, `i16`, `i32`, `i64`
- Unsigned integers: `u8`, `u16`, `u32`, `u64`
- Floats: `f32`, `f64`
- Character: `char`

## Collections

- Object collection with `string` keys and `number` values:
  - Shorthand: `field: { object: { string: 'number' } }`
  - Long form: `field: { type: 'object', key: 'string', value: 'number' }`
- Array of number, maximum length of 10 elements:
  - Long form: `field: { type: 'array', value: 'number', length: 10 }`
  - Shorthand: `field: { array: 'number', length: 10 }`
- Array of string, length between 10 and 20 elements:
  - Shorthand: `field: { array: 'number', length: { min: 10, max: 20 } }`
- Set of numbers:
  - Shorthand: `field: { set: 'number' }`
  - Long form: `field: { type: 'set', value: 'number' }`
- Map with `string` keys and `number` values:
  - Shorthand: `field: { string: 'number' }`
  - Long form: `field: { type: 'map', key: 'string', value: 'number' }`
- Tuples
  - Tuple of 3 numbers: `field: { type: 'tuple', value: 'number', length: 3 }`
  - Tuple of two elements (number and string) `field: ['string', 'number']`

## Enumerated

- Value of enumerated type:
  - Shorthand: `field: { enum: ['uno', 'due', 'tre'] }`
  - Long form: `field: { type: 'enum', enum: ['uno', 'due', 'tre'] }`
- Array of enumerated type:
  - Shorthand: `field: { array: { enum: ['uno', 'due', 'tre'] } }`
  - Long form: `field: { type: 'array', enum: ['uno', 'due', 'tre'] }`
- Map of enumerated type values and string keys:
  - Shorthand: `field: { map: { key: 'string', enum: [1, 2, 3] } }`
  - Long form: `field: { type: 'map', key: 'string' enum: [1, 2, 3] }`

## Complex types

- Domain (complex type defined in this model with UperrCamalCase name):
  `field: 'DomainName'` or `field: { type: 'DomainName' }`
