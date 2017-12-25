# cordova-Nano-SQLite

The most powerful little database, now in your mobile device!

<img src="https://raw.githubusercontent.com/ClickSimply/Nano-SQL/master/logo.png" alt="nanoSQL Logo">

What if your app's data store and permenent storage where one and the same?  NanoSQL is the quickest way to get SQL power into your app. You get tons of RDBMS perks like joins, groupby, functions and orderby with strong runtime type casting, events, and caching.  

This is a special plugin written for NanoSQL that uses IndexedDB/WebSQL in the browser for testing, then switches over to a full SQLite database on the device *with exactly the same API!*  Test all day long in the browser, then get a full real database on the device with **zero** effort.

* [Todo Example](https://nanosql.io/react-todo/)
* [Draw Example](https://nanosql.io/react-draw/)

[Documentation](https://docs.nanosql.io/)

## Highlighted Features
- Works on device and in the browser with same API.
- Supports all common RDBMS queries.
- Import and Export CSV/JSON.
- **Simple & elegant undo/redo.**
- Full Typescript support.
- **Runtime type casting.**
- **Complete ORM support.**
- Fast secondary indexes.
- Full events system.


## Installation

```sh
cordova plugin add cordova-plugin-nano-sqlite --save
```

## Simple Usage

Currently only supports use with Webpack and other similar bundlers.  Works with Babel, ES5 and Typescript projects out of the box.

```ts
import { initNanoSQL } from "cordova-plugin-nano-sqlite";

document.addEventListener(cordova ? "deviceready" : "DOMContentLoaded", () => {
  
    new initNanoSQL(
        (nSQL) => {
            // setup the database, no queries here...
            nSQL('users') //  "users" is our table name.
            .model([ // Declare data model
                {key:'id',type:'int',props:['pk','ai']}, // pk == primary key, ai == auto incriment
                {key:'name',type:'string'},
                {key:'age', type:'int'}
            ])
            .config({ // all config parameters work except "mode" which is overwritten by the plugin.
                id: "myApp", // used as SQLite database name
                history: true
            })
        }
    ).connect().then((result, nSQL) => {
        // window.nSQL() will now also work, we can do queries now!

        nSQL("users")
        .query('upsert',{ // Add a record
                name:"bill", age: 20
        }).exec()
        .then((result, db) => {
            return db.query('select').exec(); // select all rows from the current active table
        }).then((result, db) => {
            console.log(result) // <= [{id:1, name:"bill", age: 20}]
        })
    });

});

```

## Multiple Databases

You can prevent the plugin from setting the window.nSQL variable, allowing you to make multiple database instances that are scoped as you need them to be.

Updating the previouse example:

```ts
import { initNanoSQL } from "cordova-plugin-nano-sqlite";

document.addEventListener(cordova ? "deviceready" : "DOMContentLoaded", () => {
  
    new initNanoSQL(
        (nSQL) => {
            // setp the database, no queries here...
        },
        true // second optional argument prevents window.nSQL from being set.
    ).connect().then((result, nSQL) => {
        // window.nSQL() will NOT work.
        // You must access nSQL using the argument passed into this function.
    });

});

```

## Advanced Usage

Basically, this plugin is just a NanoSQL Adapter for the SQLite Cordova plugin with a small wrapper.  The code above handles switching between the different database modes for you to make things easier, but if you need extra control you can just use the adapter directly.

```ts
import { SQLiteStore } from "cordova-plugin-nano-sqlite/lib/sqlite-adapter";

document.addEventListener(typeof cordova !== "undefined" ? "deviceready" : "DOMContentLoaded", () => {
  
    nSQL("table")
    .model([...])
    .config({
        mode: cordova ? new SQLiteStore() : "PERM"
    })
    .connect().then....

});
```

And that's it, you can use this just like you would use NanoSQL in the browser or anywhere else.  The API is exactly the same.


## More detailed use cases, examples and documentation: 
[Documentation](https://docs.nanosql.io/)

# MIT License

Copyright (c) 2017 Scott Lott

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.