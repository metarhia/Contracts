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
For example, `City.js` definition may look like:
```js
({
  Registry: { realm: 'application', storage: 'append' },

  country: 'Country',
  name: { type: 'string', unique: true },
  location: { type: 'point', required: false },
  population: { type: 'number', default: 0 },

  area: { // group of fields
    total: '?number', // nullable field (shorthand for required: false)
    water: '?number',
  },
});
```

Here `Registry` is a metadata record. Other allowed kinds are:
- `Dictionary` - lookup table, have own id for primary key;
- `Registry` - registry, uses global identifier for primary key;
- `Entity` - entity, has own id for primary key;
- `Journal` - access logs and other logs, have own id for primary key);
- `Details` - details for registry or entity table, have own id for primary key;
- `Relation` - details attached to the intersection of multiple entities;
- `View` - named query database allow to select from as from regular table;
- `Struct` - structure (in-memory, API contract or db complex data type);
- `Scalar` - scalar value;
- `Form` - user interface form;
- `Projection` - schema projection;

Realm:
- `global` - universal and globally used data;
- `application` - application specific data;
- `local` - data stored on this certain server;

Storage: `enum: ['persistent', 'append', 'view', 'memory'] }`.

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
