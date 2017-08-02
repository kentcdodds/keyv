<h1 align="center">
	<img width="250" src="https://rawgit.com/lukechilds/keyv/master/media/logo.svg" alt="keyv">
	<br>
	<br>
</h1>

> Simple key-value storage with support for multiple backends

[![Build Status](https://travis-ci.org/lukechilds/keyv.svg?branch=master)](https://travis-ci.org/lukechilds/keyv)
[![Coverage Status](https://coveralls.io/repos/github/lukechilds/keyv/badge.svg?branch=master)](https://coveralls.io/github/lukechilds/keyv?branch=master)
[![npm](https://img.shields.io/npm/v/keyv.svg)](https://www.npmjs.com/package/keyv)

Keyv is a simple key-value storage module with support for multiple backends via storage adapters. Supports TTL based expiry making it suitable as a cache or a persistent key-value store.

## Features

There are a few existing modules similar to Keyv, however none of them covered all of these use cases:

- Simple Promise based API
- Suitable as cache or persistent key-value store
- Works with any storage that implements the [`Map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) API
- TTL based expiry
- Handles all JavaScript types (values can be `Buffer`/`null`/`undefined`)
- Supports namespaces
- Wide range of **efficient, well tested** storage adapters
- Connection errors are passed through (db failures won't kill your app)
- Supports the latest active LTS version of Node.js

## Usage

Install Keyv.

```
npm install --save keyv
```

By default everything is stored in memory, you can optionally also install a storage adapter.

```
npm install --save keyv-redis
npm install --save keyv-mongo
npm install --save keyv-sqlite
npm install --save keyv-postgres
npm install --save keyv-mysql
```

Create a new Keyv instance, passing your connection string if applicable. That's it!

```js
const Keyv = require('keyv');

// One of the following
const keyv = new Keyv();
const keyv = new Keyv('redis://user:pass@localhost:6379');
const keyv = new Keyv('mongodb://user:pass@localhost:27017/dbname');
const keyv = new Keyv('sqlite://path/to/database.sqlite');
const keyv = new Keyv('postgresql://user:pass@localhost:5432/dbname');
const keyv = new Keyv('mysql://user:pass@localhost:3306/dbname');

// Handle DB connection errors
keyv.on('error' err => console.log('Connection Error', err));

await keyv.set('foo', 'expires in 1 second', 1000); // true
await keyv.set('foo', 'never expires'); // true
await keyv.get('foo'); // 'never expires'
await keyv.delete('foo'); // true
await keyv.clear(); // undefined
```

## License

MIT © Luke Childs
