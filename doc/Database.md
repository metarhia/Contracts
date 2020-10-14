# Database schema

Directory with database schema should contain following files:
| File              | Description       |
| ----------------- | ----------------- |
| `.database.js`    | Schema parameters |
| `.types.js`       | Custom types      |
| `<EntityName>.js` | Entity definition |
|                   |                   |

## Schema parameters `.database.js`

```js
({
  name: 'example',
  description: 'Example database schema',
  version: 3,
  driver: 'pg',

  authors: [
    { name: 'Timur Shemsedinov', email: 'timur.shemsedinov@gmail.com' },
  ],

  extensions: [
    'hstore',
    'postgis',
    'postgis_topology',
    'pg_trgm',
  ],

  connection: {
    host: '127.0.0.1',
    port: 5432,
    database: 'application',
    user: 'postgres',
    password: 'postgres',
  },
});
```

## Custom types `.types.js`

```js
({
  point: 'geometry(Point, 4326)',
});
```

## Entity definition `<EntityName>.js`

Where `<EntityName>` is a name of certain Entity (Class) of subject domain.
For example, `City.js` definition may looks like:
```js
({
  City: 'global dictionary',

  country: 'Country',
  name: { type: 'string', unique: true },
  location: { type: 'point', required: false },
  population: { type: 'number', default: 0 },
});
```

Here is `Country` is a different entity referred from this one.

Scope: `'global'`, `'system'`, `'local'`, `'memory'`.

Kind: `'dictionary'`, `'registry'`, `'entity'`, `'log'`, `'details'`,
`'relation'`, `'struct'`, `'form'`, `'view'`, `'projection'`.

## Composite, complex and custom index

Define custom primary key:
```js
({
  company: 'Company',
  city: 'City',

  companyCity: { primary: ['Company', 'City'] },
});
// This will generate pkCompanyCity
```

Many-to-many relation (Company-to-City):
```js
// File: Company.js
({
  name: 'string',
  cities: { many: 'City' },
});
// This will generate CompanyCities cross-reference table
// with structure: { company, city }
```

Unique composite index:
```js
({
  country: 'Country',
  name: 'string',

  nameByCountry: { unique: ['country', 'name'] },
});
```

Not unique composite index:
```js
({
  street: 'string',
  building: 'string',
  apartment: 'string',

  natural: { index: ['street', 'building', 'apartment'] },
});
```

Custom indexes (gin, gist):
```js
({
  name: 'string',
  location: 'point',

  akName: { index: 'gin (name gin_trgm_ops)' },
  akLocation: { index: 'gist (location)' },
});
```
