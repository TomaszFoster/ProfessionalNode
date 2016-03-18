#### CommonJS: A format optimized for the Server ####

##### From [Writing Modular JavaScript](https://addyosmani.com/writing-modular-js/)

CommonJS is a volunteer working group aiming to design, prototype, and standardize JavaScript APIs. To date they've proposed standards for both Modules and Packages.

At a high level, CJS modules contain two parts, a free variable named `exports` which contains the objects we wish to make available to other modules and a `require` function that modules can use to import the exports of other modules.

```js
// package/lib is a dependency we require
var lib = require('package/lib');

// some behaviour for our module
function foo(){
    lib.log('hello world!');
}

// export (expose) foo to other modules
exports.foo = foo;
```

Basic consumption of exports
```js
// define more behaviour we would like to expose
function foobar(){
        this.foo = function(){
                console.log('Hello foo');
        }

        this.bar = function(){
                console.log('Hello bar');
        }
}

// expose foobar to other modules
exports.foobar = foobar;


// an application consuming 'foobar'

// access the module relative to the path
// where both usage and module files exist
// in the same directory

var foobar = require('./foobar').foobar,
    test   = new foobar();

test.bar(); // 'Hello bar'
```