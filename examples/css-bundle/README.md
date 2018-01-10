
# example.js

``` javascript
require("./style.css");
```

# style.css

``` css
body {
	background: url(image.png);
}
```

# webpack.config.js

``` javascript
const LoaderOptionsPlugin = require("../../lib/LoaderOptionsPlugin");
const ExtractTextPlugin = require("extract-text-webpack-plugin");
module.exports = {
	// mode: "development" || "production",
	module: {
		rules: [
			{
				test: /\.css$/,
				use: ExtractTextPlugin.extract({
					use: "css-loader"
				})
			},
			{ test: /\.png$/, loader: "file-loader" }
		]
	},
	plugins: [
		new ExtractTextPlugin({
			filename: "style.css",
			allChunks: true
		}),
		// Temporary workaround for the file-loader
		new LoaderOptionsPlugin({
			options: {}
		})
	]
};
```

# dist/output.js

<details><summary><code>/******/ (function(modules) { /* webpackBootstrap */ })</code></summary>

``` javascript
/******/ (function(modules) { // webpackBootstrap
/******/ 	// The module cache
/******/ 	var installedModules = {};
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
/******/ 	// Load entry module and return exports
/******/ 	return __webpack_require__(__webpack_require__.s = 0);
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

__webpack_require__(/*! ./style.css */ 1);

/***/ }),
/* 1 */
/*!*******************!*\
  !*** ./style.css ***!
  \*******************/
/*! no static exports found */
/***/ (function(module, exports) {

// removed by extract-text-webpack-plugin

/***/ })
/******/ ]);
```

# dist/style.css

``` css
body {
	background: url(dist/ce21cbdd9b894e6af794813eb3fdaf60.png);
}
```

# Info

## Unoptimized

```
Hash: 0a1b2c3d4e5f6a7b8c9d
Version: webpack next
                               Asset       Size  Chunks             Chunk Names
ce21cbdd9b894e6af794813eb3fdaf60.png  119 bytes          [emitted]  
                           output.js   2.97 KiB       0  [emitted]  main
                           style.css   71 bytes       0  [emitted]  main
Entrypoint main = output.js style.css
chunk    {0} output.js, style.css (main) 64 bytes [entry] [rendered]
    > main [0] ./example.js 
    [0] ./example.js 23 bytes {0} [built]
        single entry .\example.js  main
    [1] ./style.css 41 bytes {0} [built]
        cjs require ./style.css [0] ./example.js 1:0-22
Child extract-text-webpack-plugin ../../node_modules/extract-text-webpack-plugin/dist ../../node_modules/css-loader/index.js!style.css:
                                   Asset       Size  Chunks             Chunk Names
    ce21cbdd9b894e6af794813eb3fdaf60.png  119 bytes          [emitted]  
    Entrypoint undefined = extract-text-webpack-plugin-output-filename
    chunk    {0} extract-text-webpack-plugin-output-filename 2.51 KiB [entry] [rendered]
        > [0] (webpack)/node_modules/css-loader!./style.css 
        [0] (webpack)/node_modules/css-loader!./style.css 231 bytes {0} [built]
            single entry !!(webpack)\node_modules\css-loader\index.js!.\style.css 
        [2] ./image.png 82 bytes {0} [built]
            cjs require ./image.png [0] (webpack)/node_modules/css-loader!./style.css 6:58-80
         + 1 hidden module
```

## Production mode

```
Hash: 0a1b2c3d4e5f6a7b8c9d
Version: webpack next
                               Asset       Size  Chunks             Chunk Names
ce21cbdd9b894e6af794813eb3fdaf60.png  119 bytes          [emitted]  
                           output.js  572 bytes       0  [emitted]  main
                           style.css   71 bytes       0  [emitted]  main
Entrypoint main = output.js style.css
chunk    {0} output.js, style.css (main) 64 bytes [entry] [rendered]
    > main [1] ./example.js 
    [0] ./style.css 41 bytes {0} [built]
        cjs require ./style.css [1] ./example.js 1:0-22
    [1] ./example.js 23 bytes {0} [built]
        single entry .\example.js  main
Child extract-text-webpack-plugin ../../node_modules/extract-text-webpack-plugin/dist ../../node_modules/css-loader/index.js!style.css:
                                   Asset       Size  Chunks             Chunk Names
    ce21cbdd9b894e6af794813eb3fdaf60.png  119 bytes          [emitted]  
    Entrypoint undefined = extract-text-webpack-plugin-output-filename
    chunk    {0} extract-text-webpack-plugin-output-filename 2.51 KiB [entry] [rendered]
        > [2] (webpack)/node_modules/css-loader!./style.css 
        [0] ./image.png 82 bytes {0} [built]
            cjs require ./image.png [2] (webpack)/node_modules/css-loader!./style.css 6:58-80
        [2] (webpack)/node_modules/css-loader!./style.css 231 bytes {0} [built]
            single entry !!(webpack)\node_modules\css-loader\index.js!.\style.css 
         + 1 hidden module
```