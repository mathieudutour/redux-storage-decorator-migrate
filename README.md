# redux-storage-decorator-migrate

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

### Usage with `redux-storage-decorator-filter` and other decorators

```js
import * as storage from 'redux-storage'
import createEngine from 'redux-storage-engine-localstorage'
import filter from 'redux-storage-decorator-filter'
import debounce from 'redux-storage-decorator-debounce'
import migrate from 'redux-storage-decorator-migrate'

const reducer = storage.reducer(reducers)
const stateVersionProp = '_stateVersion'
const whitelist = [stateVersionProp]
const blacklist = []
const engine = migrate(
  debounce(
    filter(
      createEngine('your.storage.identifier'),
      whitelist,
      blacklist
    ), 1000
  ), 0
)

// Your first migration:
engine.addMigration(1, (state) => { /* migration step for 1 */ return state; });
```

## Testing migrations without a store (applying against ad-hoc state)

```js
import {buildMigrationEngine} from 'redux-storage-decorator-migrate'
const versionKey = 'redux-storage-decorators-migrate-version'
 
const someTestState = {
  [versionKey]: 0,
  myFancyStateProperty: 'A'
}

const someExampleMigration = {
  version: 1,
  migration: (state) => ({...state, myFancyStateProperty: 'B'})
}

const migrationEngine = buildMigrationEngine(1, versionKey, [someExampleMigration])

const migratedState = migrationEngine(someTestState)

console.log(migratedState.myFancyStateProperty)
// B
```

## Advanced Usage

Handling your migration engine explicitly:

```js
import migrate, {buildMigrationEngine} from 'redux-storage-decorator-migrate'
const versionKey = 'redux-storage-decorators-migrate-version'

const migrations = [{
  version: 1,
  migration: (state) => ({ /* migration step for 1 */ return state; })
}, {
  version: 2,
  migration: (state) => ({ /* migration step for 2 */ return state; })
}]

const migrationEngine = buildMigrationEngine(1, versionKey, migrations)

engine = migrate(engine, 3, versionKey, migrations, migrationEngine);
engine.addMigration(3, (state) => { /* migration step for 3 */ return state; }); // still possible

// Migrate redux's initialState if provided
const migratedInitialState = migrationEngine(initialState || {});

// Redux store creation
const reduxStore = createStore(reduxStorage.reducer(combineReducers(reducers)), migratedInitialState, compose(...enhancers));
```

## License

  MIT

  [redux-storage]: https://github.com/michaelcontento/redux-storage
  [redux-storage-decorator-migrate]: https://github.com/mathieudutour/redux-storage-decorator-migrate
