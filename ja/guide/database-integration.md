---
layout: page
title: Express でのデータベースの統合
menu: guide
lang: ja
---

# データベースの統合

データベースを Express アプリケーションに接続できるようにするには、単にデータベースに適切な Node.js ドライバーをアプリケーションにロードするだけですみます。本書では、データベース・システム用の最も一般的な Node.js モジュールを Express アプリケーションに追加して使用する方法について簡単に説明します。

* [Cassandra](#cassandra)
* [CouchDB](#couchdb)
* [LevelDB](#leveldb)
* [MySQL](#mysql)
* [MongoDB](#mongo)
* [Neo4j](#neo4j)
* [PostgreSQL](#postgres)
* [Redis](#redis)
* [SQLite](#sqlite)
* [ElasticSearch](#elasticsearch)

<div class="doc-box doc-notice" markdown="1">
上記は、使用可能な多数のデータベース・ドライバーの一部です。その他のオプションについては、[npm](https://www.npmjs.com/) サイトで検索してください。
</div>

<a name="cassandra"></a>

## Cassandra

**モジュール**: [cassandra-driver](https://github.com/datastax/nodejs-driver)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install cassandra-driver
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var cassandra = require('cassandra-driver');
var client = new cassandra.Client({ contactPoints: ['localhost']});

client.execute('select key from system.local', function(err, result) {
  if (err) throw err;
  console.log(result.rows[0]);
});
</code>
</pre>

<a name="couchdb"></a>

## CouchDB

**モジュール**: [nano](https://github.com/dscape/nano)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install nano
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var nano = require('nano')('http://localhost:5984');
nano.db.create('books');
var books = nano.db.use('books');

//Insert a book document in the books database
books.insert({name: 'The Art of war'}, null, function(err, body) {
  if (!err){
    console.log(body);
  }
});

//Get a list of all books
books.list(function(err, body){
  console.log(body.rows);
});
</code>
</pre>

<a name="leveldb"></a>

## LevelDB

**モジュール**: [levelup](https://github.com/rvagg/node-levelup)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install level levelup leveldown
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var levelup = require('levelup');
var db = levelup('./mydb');

db.put('name', 'LevelUP', function (err) {

  if (err) return console.log('Ooops!', err);
  db.get('name', function (err, value) {
    if (err) return console.log('Ooops!', err);
    console.log('name=' + value);
  });

});
</code>
</pre>

<a name="mysql"></a>

## MySQL

**モジュール**: [mysql](https://github.com/felixge/node-mysql/)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install mysql
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'dbuser',
  password : 's3kreee7'
});

connection.connect();

connection.query('SELECT 1 + 1 AS solution', function(err, rows, fields) {
  if (err) throw err;
  console.log('The solution is: ', rows[0].solution);
});

connection.end();
</code>
</pre>

<a name="mongo"></a>

## MongoDB

**モジュール**: [mongodb](https://github.com/mongodb/node-mongodb-native)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install mongodb
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var MongoClient = require('mongodb').MongoClient;

MongoClient.connect('mongodb://localhost:27017/animals', function(err, db) {
  if (err) {
    throw err;
  }
  db.collection('mammals').find().toArray(function(err, result) {
    if (err) {
      throw err;
    }
    console.log(result);
  });
});
</code>
</pre>

MongoDB 用のオブジェクト・モデル・ドライバーが必要な場合は、[Mongoose](https://github.com/LearnBoost/mongoose) を参照してください。

<a name="neo4j"></a>

## Neo4j

**モジュール**: [apoc](https://github.com/hacksparrow/apoc)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install apoc
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var apoc = require('apoc');

apoc.query('match (n) return n').exec().then(
  function (response) {
    console.log(response);
  },
  function (fail) {
    console.log(fail);
  }
);
</code>
</pre>

<a name="postgres"></a>

## PostgreSQL

**モジュール**: [pg-promise](https://github.com/vitaly-t/pg-promise)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install pg-promise
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var pgp = require("pg-promise")(/*options*/);
var db = pgp("postgres://username:password@host:port/database");

db.one("SELECT $1 AS value", 123)
    .then(function (data) {
        console.log("DATA:", data.value);
    })
    .catch(function (error) {
        console.log("ERROR:", error);
    });
</code>
</pre>

<a name="redis"></a>

## Redis

**モジュール**: [redis](https://github.com/mranney/node_redis)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install redis
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var client = require('redis').createClient();

client.on('error', function (err) {
  console.log('Error ' + err);
});

client.set('string key', 'string val', redis.print);
client.hset('hash key', 'hashtest 1', 'some value', redis.print);
client.hset(['hash key', 'hashtest 2', 'some other value'], redis.print);

client.hkeys('hash key', function (err, replies) {

  console.log(replies.length + ' replies:');
  replies.forEach(function (reply, i) {
    console.log('    ' + i + ': ' + reply);
  });

  client.quit();

});
</code>
</pre>

<a name="sqlite"></a>

## SQLite

**モジュール**: [sqlite3](https://github.com/mapbox/node-sqlite3)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install sqlite3
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var sqlite3 = require('sqlite3').verbose();
var db = new sqlite3.Database(':memory:');

db.serialize(function() {

  db.run('CREATE TABLE lorem (info TEXT)');
  var stmt = db.prepare('INSERT INTO lorem VALUES (?)');

  for (var i = 0; i < 10; i++) {
    stmt.run('Ipsum ' + i);
  }

  stmt.finalize();

  db.each('SELECT rowid AS id, info FROM lorem', function(err, row) {
    console.log(row.id + ': ' + row.info);
  });
});

db.close();
</code>
</pre>

<a name="elasticsearch"></a>

## ElasticSearch

**モジュール**: [elasticsearch](https://github.com/elastic/elasticsearch-js)
**インストール**

<pre>
<code class="language-sh" translate="no">
$ npm install elasticsearch
</code>
</pre>

**例**

<pre>
<code class="language-javascript" translate="no">
var elasticsearch = require('elasticsearch');
var client = elasticsearch.Client({
  host: 'localhost:9200'
});

client.search({
  index: 'books',
  type: 'book',
  body: {
    query: {
      multi_match: {
        query: 'express js',
        fields: ['title', 'description']
      }
    }
  }
}).then(function(response) {
  var hits = response.hits.hits;
}, function(error) {
  console.trace(error.message);
});
</code>
</pre>
