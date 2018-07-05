## Description:
This package help you to convert csv/tsv files to js object and write it to sqlite database.

## Installation:
```
npm install tsv-to-sqlite --save
```

## Example of usage:

This example read stream from tsv file and write it to sqlite database
```javascript
const fs = require('fs');
const {
  FilterTransformStream,
  TsvToJsonTransformStream,
  ProgressTransformStream,
  SqliteWriteStream,
} = require('tsv-to-sqlite');

const tsvFilePath = './cal.tsv';
const dbName = './cal.db';

const fileSize = fs.statSync(tsvFilePath).size;

fs.createReadStream(tsvFilePath)
  .pipe(new ProgressTransformStream(fileSize))
  .pipe(new TsvToJsonTransformStream())
  .pipe(new SqliteWriteStream(dataBaseName));
```

This example open download stream from imdb and write it at database. Filter function is used to filter only movies from stream. 
```javascript
const zlib = require('zlib');
const request = require('request');

const {
  FilterTransformStream,
  TsvToJsonTransformStream,
  ProgressTransformStream,
  SqliteWriteStream,
} = require('tsv-to-sqlite');

const dbName = './movie.db';
const filter = obj => obj.titleType === 'movie';

request.get('https://datasets.imdbws.com/title.basics.tsv.gz').on('response', response => {
  const size = Number(response.headers['content-length']);
  response
    .pipe(new ProgressTransformStream(size))
    .pipe(zlib.createGunzip())
    .pipe(new TsvToJsonTransformStream())
    .pipe(new FilterTransformStream(filter))
    .pipe(new SqliteWriteStream(dbName, { tableName: 'basic' }));
});
```

## Documentation

### TsvToJsonTransformStream
coming soon...

### SqliteWriteStream
coming soon...

### ProgressTransformStream
coming soon...

### FilterTransformStream
coming soon...
