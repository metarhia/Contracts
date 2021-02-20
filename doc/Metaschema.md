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
  `field: { object: { string: 'number' } }`
- Array of number, maximum lenght of 10 elements:
  `field: { array: 'number', length: 10 }`
- Array of string, lenght betweeb 10 and 20 elements:
  `field: { array: 'number', length: { min: 10, max: 20 } }`
- Tuple of two numbers:
  `field: ['number', 'number']`
- Set of numbers:
  `fieeld: { set: 'number' }`
- Map with `string` keys and `number` values:
  `{ map: { string: 'number' } }`

## Complex types

- Domain (complex type defined in this model with UperrCamalCase name):
  `field: 'DomainName'` or `field: { type: 'DomainName' }`
