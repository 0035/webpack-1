This example illustrates how common modules from deep ancestors of an entry point can be split into a seperate common chunk

* `pageA` and `pageB` are dynamically required
* `pageC` and `pageA` both require the `reusableComponent`
* `pageB` dynamically requires `PageC`

You can see that webpack outputs four files/chunks:

* `output.js` is the entry chunk and contains
  * the module system
  * chunk loading logic
  * the entry point `example.js`
  * module `reusableComponent`
* `0.output.js` is an additional chunk
  * module `pageC`
* `1.output.js` is an additional chunk
  * module `pageB`
* `2.output.js` is an additional chunk
  * module `pageA`


# example.js

``` javascript
var main = function() {
	console.log("Main class");
	require.ensure([], () => {
		const page = require("./pageA");
		page();
	});
	require.ensure([], () => {
		const page = require("./pageB");
		page();
	});
};

main();
```

# pageA.js

``` javascript
var reusableComponent = require("./reusableComponent");

module.exports = function() {
	console.log("Page A");
	reusableComponent();
};
```

# pageB.js

``` javascript
module.exports = function() {
	console.log("Page B");
	require.ensure([], ()=>{
		const page = require("./pageC");
		page();
	});
};
```

# pageC.js

``` javascript
var reusableComponent = require("./reusableComponent");

module.exports = function() {
	console.log("Page C");
	reusableComponent();
};
```

# reusableComponent.js

``` javascript
module.exports = function() {
	console.log("reusable Component");
};
```

# webpack.config.js

``` javascript
"use strict";
const webpack = require("../../");
const path = require("path");

module.exports = [
	{
		// mode: "development || "production",
		entry: {
			main: ["./example.js"]
		},
		output: {
			path: path.resolve(__dirname, "dist"),
			filename: "output.js"
		},
		plugins: [
			new webpack.optimize.CommonsChunkPlugin({
				name: "main",
				minChunks: 2,
				children: true,
				deepChildren: true,
			})
		],
		optimization: {
			occurrenceOrder: true // To keep filename consistent between different modes (for example building only)
		}
	},
	{
		// mode: "development || "production",
		entry: {
			main: ["./example.js"]
		},
		output: {
			path: path.resolve(__dirname, "dist"),
			filename: "asyncoutput.js"
		},
		plugins: [
			new webpack.optimize.CommonsChunkPlugin({
				name: "main",
				minChunks: 2,
				async: true,
				children: true,
				deepChildren: true,
			})
		],
		optimization: {
			occurrenceOrder: true // To keep filename consistent between different modes (for example building only)
		}
	}
];
```

# dist/output.js

<details><summary><code>/******/ (function(modules) { /* webpackBootstrap */ })</code></summary>

``` javascript
/******/ (function(modules) { // webpackBootstrap
/******/ 	// install a JSONP callback for chunk loading
/******/ 	function webpackJsonpCallback(data) {
/******/ 		var chunkIds = data[0], moreModules = data[1], executeModules = data[2];
/******/ 		// add "moreModules" to the modules object,
/******/ 		// then flag all "chunkIds" as loaded and fire callback
/******/ 		var moduleId, chunkId, i = 0, resolves = [], result;
/******/ 		for(;i < chunkIds.length; i++) {
/******/ 			chunkId = chunkIds[i];
/******/ 			if(installedChunks[chunkId]) {
/******/ 				resolves.push(installedChunks[chunkId][0]);
/******/ 			}
/******/ 			installedChunks[chunkId] = 0;
/******/ 		}
/******/ 		for(moduleId in moreModules) {
/******/ 			if(Object.prototype.hasOwnProperty.call(moreModules, moduleId)) {
/******/ 				modules[moduleId] = moreModules[moduleId];
/******/ 			}
/******/ 		}
/******/ 		if(parentJsonpFunction) parentJsonpFunction(data);
/******/ 		while(resolves.length) {
/******/ 			resolves.shift()();
/******/ 		}
/******/
/******/ 	};
/******/
/******/ 	// The module cache
/******/ 	var installedModules = {};
/******/
/******/ 	// object to store loaded and loading chunks
/******/ 	var installedChunks = {
/******/ 		3: 0
/******/ 	};
/******/
/******/ 	var scheduledModules = [];
/******/
/******/ 	// The require function
/******/ 	function __webpack_require__(moduleId) {
/******/
/******/ 		// Check if module is in cache
/******/ 		if(installedModules[moduleId]) {
/******/ 			return installedModules[moduleId].exports;
/******/ 		}
/******/ 		// Create a new module (and put it into the cache)
/******/ 		var module = installedModules[moduleId] = {
/******/ 			i: moduleId,
/******/ 			l: false,
/******/ 			exports: {}
/******/ 		};
/******/
/******/ 		// Execute the module function
/******/ 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
/******/
/******/ 		// Flag the module as loaded
/******/ 		module.l = true;
/******/
/******/ 		// Return the exports of the module
/******/ 		return module.exports;
/******/ 	}
/******/
/******/ 	// This file contains only the entry chunk.
/******/ 	// The chunk loading function for additional chunks
/******/ 	__webpack_require__.e = function requireEnsure(chunkId) {
/******/ 		var promises = [];
/******/
/******/
/******/ 		// JSONP chunk loading for javascript
/******/
/******/ 		var installedChunkData = installedChunks[chunkId];
/******/ 		if(installedChunkData !== 0) { // 0 means "already installed".
/******/
/******/ 			// a Promise means "currently loading".
/******/ 			if(installedChunkData) {
/******/ 				promises.push(installedChunkData[2]);
/******/ 			} else {
/******/ 				// setup Promise in chunk cache
/******/ 				var promise = new Promise(function(resolve, reject) {
/******/ 					installedChunkData = installedChunks[chunkId] = [resolve, reject];
/******/ 				});
/******/ 				promises.push(installedChunkData[2] = promise);
/******/
/******/ 				// start chunk loading
/******/ 				var head = document.getElementsByTagName('head')[0];
/******/ 				var script = document.createElement('script');
/******/
/******/ 				script.charset = 'utf-8';
/******/ 				script.timeout = 120000;
/******/
/******/ 				if (__webpack_require__.nc) {
/******/ 					script.setAttribute("nonce", __webpack_require__.nc);
/******/ 				}
/******/ 				script.src = __webpack_require__.p + "" + chunkId + ".output.js";
/******/ 				var timeout = setTimeout(function(){
/******/ 					onScriptComplete({ type: 'timeout', target: script });
/******/ 				}, 120000);
/******/ 				script.onerror = script.onload = onScriptComplete;
/******/ 				function onScriptComplete(event) {
/******/ 					// avoid mem leaks in IE.
/******/ 					script.onerror = script.onload = null;
/******/ 					clearTimeout(timeout);
/******/ 					var chunk = installedChunks[chunkId];
/******/ 					if(chunk !== 0) {
/******/ 						if(chunk) {
/******/ 							var errorType = event && (event.type === 'load' ? 'missing' : event.type);
/******/ 							var realSrc = event && event.target && event.target.src;
/******/ 							var error = new Error('Loading chunk ' + chunkId + ' failed.\n(' + errorType + ': ' + realSrc + ')');
/******/ 							error.type = errorType;
/******/ 							error.request = realSrc;
/******/ 							chunk[1](error);
/******/ 						}
/******/ 						installedChunks[chunkId] = undefined;
/******/ 					}
/******/ 				};
/******/ 				head.appendChild(script);
/******/ 			}
/******/ 		}
/******/ 		return Promise.all(promises);
/******/ 	};
/******/
/******/ 	// expose the modules object (__webpack_modules__)
/******/ 	__webpack_require__.m = modules;
/******/
/******/ 	// expose the module cache
/******/ 	__webpack_require__.c = installedModules;
/******/
/******/ 	// define getter function for harmony exports
/******/ 	__webpack_require__.d = function(exports, name, getter) {
/******/ 		if(!__webpack_require__.o(exports, name)) {
/******/ 			Object.defineProperty(exports, name, {
/******/ 				configurable: false,
/******/ 				enumerable: true,
/******/ 				get: getter
/******/ 			});
/******/ 		}
/******/ 	};
/******/
/******/ 	// define __esModule on exports
/******/ 	__webpack_require__.r = function(exports) {
/******/ 		Object.defineProperty(exports, '__esModule', { value: true });
/******/ 	};
/******/
/******/ 	// getDefaultExport function for compatibility with non-harmony modules
/******/ 	__webpack_require__.n = function(module) {
/******/ 		var getter = module && module.__esModule ?
/******/ 			function getDefault() { return module['default']; } :
/******/ 			function getModuleExports() { return module; };
/******/ 		__webpack_require__.d(getter, 'a', getter);
/******/ 		return getter;
/******/ 	};
/******/
/******/ 	// Object.prototype.hasOwnProperty.call
/******/ 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };
/******/
/******/ 	// __webpack_public_path__
/******/ 	__webpack_require__.p = "dist/";
/******/
/******/ 	// on error function for async loading
/******/ 	__webpack_require__.oe = function(err) { console.error(err); throw err; };
/******/
/******/ 	var jsonpArray = window["webpackJsonp"] = window["webpackJsonp"] || [];
/******/ 	var parentJsonpFunction = jsonpArray.push.bind(jsonpArray);
/******/ 	jsonpArray.push = webpackJsonpCallback;
/******/ 	jsonpArray = jsonpArray.slice();
/******/ 	for(var i = 0; i < jsonpArray.length; i++) webpackJsonpCallback(jsonpArray[i]);
/******/
/******/ 	// Load entry module and return exports
/******/ 	return __webpack_require__(__webpack_require__.s = 1);
/******/ })
/************************************************************************/
```

</details>

``` javascript
/******/ ([
/* 0 */
/*!********************!*\
  !*** ./example.js ***!
  \********************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

var main = function() {
	console.log("Main class");
	__webpack_require__.e(/*! require.ensure */ 2).then((() => {
		const page = __webpack_require__(/*! ./pageA */ 4);
		page();
	}).bind(null, __webpack_require__)).catch(__webpack_require__.oe);
	__webpack_require__.e(/*! require.ensure */ 1).then((() => {
		const page = __webpack_require__(/*! ./pageB */ 3);
		page();
	}).bind(null, __webpack_require__)).catch(__webpack_require__.oe);
};

main();


/***/ }),
/* 1 */
/*!**************************!*\
  !*** multi ./example.js ***!
  \**************************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

module.exports = __webpack_require__(/*! ./example.js */0);


/***/ }),
/* 2 */
/*!******************************!*\
  !*** ./reusableComponent.js ***!
  \******************************/
/*! no static exports found */
/***/ (function(module, exports) {

module.exports = function() {
	console.log("reusable Component");
};


/***/ })
/******/ ]);
```

# dist/0.output.js

``` javascript
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[0],{

/***/ 5:
/*!******************!*\
  !*** ./pageC.js ***!
  \******************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

var reusableComponent = __webpack_require__(/*! ./reusableComponent */ 2);

module.exports = function() {
	console.log("Page C");
	reusableComponent();
};


/***/ })

}]);
```

# dist/1.output.js

``` javascript
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[1],{

/***/ 3:
/*!******************!*\
  !*** ./pageB.js ***!
  \******************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

module.exports = function() {
	console.log("Page B");
	__webpack_require__.e(/*! require.ensure */ 0).then((()=>{
		const page = __webpack_require__(/*! ./pageC */ 5);
		page();
	}).bind(null, __webpack_require__)).catch(__webpack_require__.oe);
};


/***/ })

}]);
```

# dist/2.output.js

``` javascript
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[2],{

/***/ 4:
/*!******************!*\
  !*** ./pageA.js ***!
  \******************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

var reusableComponent = __webpack_require__(/*! ./reusableComponent */ 2);

module.exports = function() {
	console.log("Page A");
	reusableComponent();
};


/***/ })

}]);
```

# dist/asyncoutput.js

``` javascript
/******/ (function(modules) { // webpackBootstrap
/******/ 	// install a JSONP callback for chunk loading
/******/ 	function webpackJsonpCallback(data) {
/******/ 		var chunkIds = data[0], moreModules = data[1], executeModules = data[2];
/******/ 		// add "moreModules" to the modules object,
/******/ 		// then flag all "chunkIds" as loaded and fire callback
/******/ 		var moduleId, chunkId, i = 0, resolves = [], result;
/******/ 		for(;i < chunkIds.length; i++) {
/******/ 			chunkId = chunkIds[i];
/******/ 			if(installedChunks[chunkId]) {
/******/ 				resolves.push(installedChunks[chunkId][0]);
/******/ 			}
/******/ 			installedChunks[chunkId] = 0;
/******/ 		}
/******/ 		for(moduleId in moreModules) {
/******/ 			if(Object.prototype.hasOwnProperty.call(moreModules, moduleId)) {
/******/ 				modules[moduleId] = moreModules[moduleId];
/******/ 			}
/******/ 		}
/******/ 		if(parentJsonpFunction) parentJsonpFunction(data);
/******/ 		while(resolves.length) {
/******/ 			resolves.shift()();
/******/ 		}
/******/
/******/ 	};
/******/
/******/ 	// The module cache
/******/ 	var installedModules = {};
/******/
/******/ 	// object to store loaded and loading chunks
/******/ 	var installedChunks = {
/******/ 		4: 0
/******/ 	};
/******/
/******/ 	var scheduledModules = [];
/******/
/******/ 	// The require function
/******/ 	function __webpack_require__(moduleId) {
/******/
/******/ 		// Check if module is in cache
/******/ 		if(installedModules[moduleId]) {
/******/ 			return installedModules[moduleId].exports;
/******/ 		}
/******/ 		// Create a new module (and put it into the cache)
/******/ 		var module = installedModules[moduleId] = {
/******/ 			i: moduleId,
/******/ 			l: false,
/******/ 			exports: {}
/******/ 		};
/******/
/******/ 		// Execute the module function
/******/ 		modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
/******/
/******/ 		// Flag the module as loaded
/******/ 		module.l = true;
/******/
/******/ 		// Return the exports of the module
/******/ 		return module.exports;
/******/ 	}
/******/
/******/ 	// This file contains only the entry chunk.
/******/ 	// The chunk loading function for additional chunks
/******/ 	__webpack_require__.e = function requireEnsure(chunkId) {
/******/ 		var promises = [];
/******/
/******/
/******/ 		// JSONP chunk loading for javascript
/******/
/******/ 		var installedChunkData = installedChunks[chunkId];
/******/ 		if(installedChunkData !== 0) { // 0 means "already installed".
/******/
/******/ 			// a Promise means "currently loading".
/******/ 			if(installedChunkData) {
/******/ 				promises.push(installedChunkData[2]);
/******/ 			} else {
/******/ 				// setup Promise in chunk cache
/******/ 				var promise = new Promise(function(resolve, reject) {
/******/ 					installedChunkData = installedChunks[chunkId] = [resolve, reject];
/******/ 				});
/******/ 				promises.push(installedChunkData[2] = promise);
/******/
/******/ 				// start chunk loading
/******/ 				var head = document.getElementsByTagName('head')[0];
/******/ 				var script = document.createElement('script');
/******/
/******/ 				script.charset = 'utf-8';
/******/ 				script.timeout = 120000;
/******/
/******/ 				if (__webpack_require__.nc) {
/******/ 					script.setAttribute("nonce", __webpack_require__.nc);
/******/ 				}
/******/ 				script.src = __webpack_require__.p + "" + chunkId + ".asyncoutput.js";
/******/ 				var timeout = setTimeout(function(){
/******/ 					onScriptComplete({ type: 'timeout', target: script });
/******/ 				}, 120000);
/******/ 				script.onerror = script.onload = onScriptComplete;
/******/ 				function onScriptComplete(event) {
/******/ 					// avoid mem leaks in IE.
/******/ 					script.onerror = script.onload = null;
/******/ 					clearTimeout(timeout);
/******/ 					var chunk = installedChunks[chunkId];
/******/ 					if(chunk !== 0) {
/******/ 						if(chunk) {
/******/ 							var errorType = event && (event.type === 'load' ? 'missing' : event.type);
/******/ 							var realSrc = event && event.target && event.target.src;
/******/ 							var error = new Error('Loading chunk ' + chunkId + ' failed.\n(' + errorType + ': ' + realSrc + ')');
/******/ 							error.type = errorType;
/******/ 							error.request = realSrc;
/******/ 							chunk[1](error);
/******/ 						}
/******/ 						installedChunks[chunkId] = undefined;
/******/ 					}
/******/ 				};
/******/ 				head.appendChild(script);
/******/ 			}
/******/ 		}
/******/ 		return Promise.all(promises);
/******/ 	};
/******/
/******/ 	// expose the modules object (__webpack_modules__)
/******/ 	__webpack_require__.m = modules;
/******/
/******/ 	// expose the module cache
/******/ 	__webpack_require__.c = installedModules;
/******/
/******/ 	// define getter function for harmony exports
/******/ 	__webpack_require__.d = function(exports, name, getter) {
/******/ 		if(!__webpack_require__.o(exports, name)) {
/******/ 			Object.defineProperty(exports, name, {
/******/ 				configurable: false,
/******/ 				enumerable: true,
/******/ 				get: getter
/******/ 			});
/******/ 		}
/******/ 	};
/******/
/******/ 	// define __esModule on exports
/******/ 	__webpack_require__.r = function(exports) {
/******/ 		Object.defineProperty(exports, '__esModule', { value: true });
/******/ 	};
/******/
/******/ 	// getDefaultExport function for compatibility with non-harmony modules
/******/ 	__webpack_require__.n = function(module) {
/******/ 		var getter = module && module.__esModule ?
/******/ 			function getDefault() { return module['default']; } :
/******/ 			function getModuleExports() { return module; };
/******/ 		__webpack_require__.d(getter, 'a', getter);
/******/ 		return getter;
/******/ 	};
/******/
/******/ 	// Object.prototype.hasOwnProperty.call
/******/ 	__webpack_require__.o = function(object, property) { return Object.prototype.hasOwnProperty.call(object, property); };
/******/
/******/ 	// __webpack_public_path__
/******/ 	__webpack_require__.p = "dist/";
/******/
/******/ 	// on error function for async loading
/******/ 	__webpack_require__.oe = function(err) { console.error(err); throw err; };
/******/
/******/ 	var jsonpArray = window["webpackJsonp"] = window["webpackJsonp"] || [];
/******/ 	var parentJsonpFunction = jsonpArray.push.bind(jsonpArray);
/******/ 	jsonpArray.push = webpackJsonpCallback;
/******/ 	jsonpArray = jsonpArray.slice();
/******/ 	for(var i = 0; i < jsonpArray.length; i++) webpackJsonpCallback(jsonpArray[i]);
/******/
/******/ 	// Load entry module and return exports
/******/ 	return __webpack_require__(__webpack_require__.s = 1);
/******/ })
/************************************************************************/
/******/ ([
/* 0 */
/*!********************!*\
  !*** ./example.js ***!
  \********************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

var main = function() {
	console.log("Main class");
	Promise.all(/*! require.ensure */[__webpack_require__.e(0), __webpack_require__.e(2)]).then((() => {
		const page = __webpack_require__(/*! ./pageA */ 3);
		page();
	}).bind(null, __webpack_require__)).catch(__webpack_require__.oe);
	__webpack_require__.e(/*! require.ensure */ 1).then((() => {
		const page = __webpack_require__(/*! ./pageB */ 2);
		page();
	}).bind(null, __webpack_require__)).catch(__webpack_require__.oe);
};

main();


/***/ }),
/* 1 */
/*!**************************!*\
  !*** multi ./example.js ***!
  \**************************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

module.exports = __webpack_require__(/*! ./example.js */0);


/***/ })
/******/ ]);
```

# dist/0.asyncoutput.js

``` javascript
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[0],{

/***/ 4:
/*!******************************!*\
  !*** ./reusableComponent.js ***!
  \******************************/
/*! no static exports found */
/***/ (function(module, exports) {

module.exports = function() {
	console.log("reusable Component");
};


/***/ })

}]);
```

# dist/1.asyncoutput.js

``` javascript
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[1],{

/***/ 2:
/*!******************!*\
  !*** ./pageB.js ***!
  \******************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

module.exports = function() {
	console.log("Page B");
	Promise.all(/*! require.ensure */[__webpack_require__.e(0), __webpack_require__.e(3)]).then((()=>{
		const page = __webpack_require__(/*! ./pageC */ 5);
		page();
	}).bind(null, __webpack_require__)).catch(__webpack_require__.oe);
};


/***/ })

}]);
```

# dist/2.asyncoutput.js

``` javascript
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[2],{

/***/ 3:
/*!******************!*\
  !*** ./pageA.js ***!
  \******************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

var reusableComponent = __webpack_require__(/*! ./reusableComponent */ 4);

module.exports = function() {
	console.log("Page A");
	reusableComponent();
};


/***/ })

}]);
```

# dist/3.asyncoutput.js

``` javascript
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[3],{

/***/ 5:
/*!******************!*\
  !*** ./pageC.js ***!
  \******************/
/*! no static exports found */
/***/ (function(module, exports, __webpack_require__) {

var reusableComponent = __webpack_require__(/*! ./reusableComponent */ 4);

module.exports = function() {
	console.log("Page C");
	reusableComponent();
};


/***/ })

}]);
```

# Info

## Unoptimized

```
Hash: 0a1b2c3d4e5f6a7b8c9d
Version: webpack next
Child
    Hash: 0a1b2c3d4e5f6a7b8c9d
          Asset       Size  Chunks             Chunk Names
    0.output.js  414 bytes       0  [emitted]  
    1.output.js  509 bytes       1  [emitted]  
    2.output.js  414 bytes       2  [emitted]  
      output.js   7.72 KiB       3  [emitted]  main
    Entrypoint main = output.js
    chunk    {0} 0.output.js 142 bytes {3} [rendered]
        > [3] ./pageB.js 3:1-6:3
        [5] ./pageC.js 142 bytes {0} [built]
            cjs require ./pageC [3] ./pageB.js 4:15-33
    chunk    {1} 1.output.js 140 bytes {3} [rendered]
        > [0] ./example.js 7:1-10:3
        [3] ./pageB.js 140 bytes {1} [built]
            cjs require ./pageB [0] ./example.js 8:15-33
    chunk    {2} 2.output.js 142 bytes {3} [rendered]
        > [0] ./example.js 3:1-6:3
        [4] ./pageA.js 142 bytes {2} [built]
            cjs require ./pageA [0] ./example.js 4:15-33
    chunk    {3} output.js (main) 333 bytes [entry] [rendered]
        > main [1] multi ./example.js 
        [0] ./example.js 233 bytes {3} [built]
            single entry ./example.js [1] multi ./example.js main:100000
        [1] multi ./example.js 28 bytes {3} [built]
            multi entry 
        [2] ./reusableComponent.js 72 bytes {3} [built]
            cjs require ./reusableComponent [4] ./pageA.js 1:24-54
            cjs require ./reusableComponent [5] ./pageC.js 1:24-54
Child
    Hash: 0a1b2c3d4e5f6a7b8c9d
               Asset       Size  Chunks             Chunk Names
    0.asyncoutput.js  340 bytes       0  [emitted]  
    1.asyncoutput.js  549 bytes       1  [emitted]  
    2.asyncoutput.js  414 bytes       2  [emitted]  
    3.asyncoutput.js  414 bytes       3  [emitted]  
      asyncoutput.js    7.5 KiB       4  [emitted]  main
    Entrypoint main = asyncoutput.js
    chunk    {0} 0.asyncoutput.js 72 bytes {4} [rendered]
        > async commons [0] ./example.js 3:1-6:3
        > async commons [2] ./pageB.js 3:1-6:3
        [4] ./reusableComponent.js 72 bytes {0} [built]
            cjs require ./reusableComponent [3] ./pageA.js 1:24-54
            cjs require ./reusableComponent [5] ./pageC.js 1:24-54
    chunk    {1} 1.asyncoutput.js 140 bytes {4} [rendered]
        > [0] ./example.js 7:1-10:3
        [2] ./pageB.js 140 bytes {1} [built]
            cjs require ./pageB [0] ./example.js 8:15-33
    chunk    {2} 2.asyncoutput.js 142 bytes {4} [rendered]
        > [0] ./example.js 3:1-6:3
        [3] ./pageA.js 142 bytes {2} [built]
            cjs require ./pageA [0] ./example.js 4:15-33
    chunk    {3} 3.asyncoutput.js 142 bytes {1} [rendered]
        > [2] ./pageB.js 3:1-6:3
        [5] ./pageC.js 142 bytes {3} [built]
            cjs require ./pageC [2] ./pageB.js 4:15-33
    chunk    {4} asyncoutput.js (main) 261 bytes [entry] [rendered]
        > main [1] multi ./example.js 
        [0] ./example.js 233 bytes {4} [built]
            single entry ./example.js [1] multi ./example.js main:100000
        [1] multi ./example.js 28 bytes {4} [built]
            multi entry 
```

## Production mode

```
Hash: 0a1b2c3d4e5f6a7b8c9d
Version: webpack next
Child
    Hash: 0a1b2c3d4e5f6a7b8c9d
          Asset       Size  Chunks             Chunk Names
    0.output.js  138 bytes       0  [emitted]  
    1.output.js  176 bytes       1  [emitted]  
    2.output.js  138 bytes       2  [emitted]  
      output.js   1.81 KiB       3  [emitted]  main
    Entrypoint main = output.js
    chunk    {0} 0.output.js 142 bytes {3} [rendered]
        > [3] ./pageB.js 3:1-6:3
        [5] ./pageC.js 142 bytes {0} [built]
            cjs require ./pageC [3] ./pageB.js 4:15-33
    chunk    {1} 1.output.js 140 bytes {3} [rendered]
        > [0] ./example.js 7:1-10:3
        [3] ./pageB.js 140 bytes {1} [built]
            cjs require ./pageB [0] ./example.js 8:15-33
    chunk    {2} 2.output.js 142 bytes {3} [rendered]
        > [0] ./example.js 3:1-6:3
        [4] ./pageA.js 142 bytes {2} [built]
            cjs require ./pageA [0] ./example.js 4:15-33
    chunk    {3} output.js (main) 333 bytes [entry] [rendered]
        > main [1] multi ./example.js 
        [0] ./example.js 233 bytes {3} [built]
            single entry ./example.js [1] multi ./example.js main:100000
        [1] multi ./example.js 28 bytes {3} [built]
            multi entry 
        [2] ./reusableComponent.js 72 bytes {3} [built]
            cjs require ./reusableComponent [4] ./pageA.js 1:24-54
            cjs require ./reusableComponent [5] ./pageC.js 1:24-54
Child
    Hash: 0a1b2c3d4e5f6a7b8c9d
               Asset       Size  Chunks             Chunk Names
    0.asyncoutput.js  133 bytes       0  [emitted]  
    1.asyncoutput.js  198 bytes       1  [emitted]  
    2.asyncoutput.js  138 bytes       2  [emitted]  
    3.asyncoutput.js  138 bytes       3  [emitted]  
      asyncoutput.js   1.76 KiB       4  [emitted]  main
    Entrypoint main = asyncoutput.js
    chunk    {0} 0.asyncoutput.js 72 bytes {4} [rendered]
        > async commons [0] ./example.js 3:1-6:3
        > async commons [2] ./pageB.js 3:1-6:3
        [4] ./reusableComponent.js 72 bytes {0} [built]
            cjs require ./reusableComponent [3] ./pageA.js 1:24-54
            cjs require ./reusableComponent [5] ./pageC.js 1:24-54
    chunk    {1} 1.asyncoutput.js 140 bytes {4} [rendered]
        > [0] ./example.js 7:1-10:3
        [2] ./pageB.js 140 bytes {1} [built]
            cjs require ./pageB [0] ./example.js 8:15-33
    chunk    {2} 2.asyncoutput.js 142 bytes {4} [rendered]
        > [0] ./example.js 3:1-6:3
        [3] ./pageA.js 142 bytes {2} [built]
            cjs require ./pageA [0] ./example.js 4:15-33
    chunk    {3} 3.asyncoutput.js 142 bytes {1} [rendered]
        > [2] ./pageB.js 3:1-6:3
        [5] ./pageC.js 142 bytes {3} [built]
            cjs require ./pageC [2] ./pageB.js 4:15-33
    chunk    {4} asyncoutput.js (main) 261 bytes [entry] [rendered]
        > main [1] multi ./example.js 
        [0] ./example.js 233 bytes {4} [built]
            single entry ./example.js [1] multi ./example.js main:100000
        [1] multi ./example.js 28 bytes {4} [built]
            multi entry 
```
