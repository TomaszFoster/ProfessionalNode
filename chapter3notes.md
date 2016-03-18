## Chapter 3 Notes

1. Loading Modules
2. Creating Modules
3. Using the `node_modules` folder

Each module has it's own context and thus cannot pollute the global namespace.

To import a module, all one needs to do is use the `require()` function like so:
```js
var fs = require('fs');
```

If you were to create a file named `circles.js` and in that file defined the following:
```js
function Circle(x, y, r) {
	function r_squared(){
		return Math.pow(r,2);
	}
	function area() {
		return Math.PI * r_squared();
	}
	return { 
		area: area
	}; 
}

module.exports = Circle;
```
The last line is the most important, it defines what we want to export to other modules.

#### Core Modules
To import a core module you reference them solely by the module name and they take priority over other modules, even if they have the same name. For example:
```js
var http = require('http');
```
#### File Modules
To load a non-core file module, you can use an absolute or relative path and omit the `.js` extension like so:
```js
var myModule = require('/home/tomasz/my_modules/myModule');
var myModule = require('/home/tomasz/my_modules/myModule.js');
var myModule = require('./myModule');
var myModule = require('./myModule.js');
```
These above are all valid ways of include file modules.

#### Folder Modules
You can load folder modules in the same way as files:
```js 
var myModule = require('./myModuleDir');
```
and Node will assume it's a package and look for a package definition `package.json`.

If one isn't found, it'll look at `index.js`.

If your folder does contain, say `./myModuleDir/package.json`, Node will try to parse this file and look for and use the `main` attribute as a relative path for entry. Thus if your `package.json` file looked like this:
```js
{
	"name" : "myModule",
	"main" : "./lib/myModule.js"
}
```
Node will then attempt to load `/myModuleDir/lib/myModule.js`

#### node_modules folder
If the module is not a core module and isn't relative, Node tries to find the module in the `node_modules` folder.

For example if you do the following:
```js
var myModule = require('myModule.js');
```
Then Node will first look for the file `/node_modules/myModule.js`, if this isn't found, then it will look for the file in the parent directory like so `./node_modules/myModule.js` and as long as it's not found, it will continue to descend through the parent directories until it reaches the root folder or finds the required module.

#### Caching Modules
Modules are cached the first time they're loaded, thus repetitive calls to `require('myModule')` returns the exact same module if the module name resolves to the same filename. This is important to remember if you're building a module that produces some side effects when it's being initialized.

