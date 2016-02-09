# redux-storage-decorator-debounce

[![build](https://travis-ci.org/mathieudutour/redux-storage-decorator-migrate.svg)](https://travis-ci.org/mathieudutour/redux-storage-decorator-migrate)
[![dependencies](https://david-dm.org/mathieudutour/redux-storage-decorator-migrate.svg)](https://david-dm.org/mathieudutour/redux-storage-decorator-migrate)
[![devDependencies](https://david-dm.org/mathieudutour/redux-storage-decorator-migrate/dev-status.svg)](https://david-dm.org/mathieudutour/redux-storage-decorator-migrate#info=devDependencies)

[![license](https://img.shields.io/npm/l/redux-storage-decorator-migrate.svg?style=flat-square)](https://www.npmjs.com/package/redux-storage-decorator-migrate)
[![npm version](https://img.shields.io/npm/v/redux-storage-decorator-migrate.svg?style=flat-square)](https://www.npmjs.com/package/redux-storage-decorator-migrate)
[![npm downloads](https://img.shields.io/npm/dm/redux-storage-decorator-migrate.svg?style=flat-square)](https://www.npmjs.com/package/redux-storage-decorator-migrate)

Migrate decorator for [redux-storage][] to version the storage with migration

## Installation

```bash
  npm install --save redux-storage-decorator-migrate
```

## Usage

Versioned storage with migrations.

```js
import migrate from 'redux-storage-decorator-migrate'

engine = migrate(engine, 3);
engine.addMigration(1, (state) => { /* migration step for 1 */ return state; });
engine.addMigration(2, (state) => { /* migration step for 2 */ return state; });
engine.addMigration(3, (state) => { /* migration step for 3 */ return state; });
```

## License

  MIT

  [redux-storage]: https://github.com/michaelcontento/redux-storage
  [redux-storage-decorator-migrate]: https://github.com/mathieudutour/redux-storage-decorator-migrate
