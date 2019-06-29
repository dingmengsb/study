---
sidebar: auto
sidebarDepth: 2
---
# js模块化规范

## CommonJS
- 用于服务端模块化编程，比如nodejs就采用此规范；

- 一个文件就是一个模块，require方法用来加载模块，该方法读取一个文件并执行，最后返回文件内部的module.exports对象；

- require是默认读取.js文件的，所以require(模块名)可以不写后缀；

- 同步加载，由于服务端加载的模块一般在本地，所以可以这样；但是在客户器端如果一个模块过大就会导致页面“假死”；

- module.exports属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取module.exports变量；为了方便也可以用exports，exports指向module.exports；即exports = module.exports = {}

- exports.xxx相当于在导出的对象上添加属性，该属性对调用模块可见；

- exports =  相当于给exports重新赋值，这样就切断了和module.exports的关联，调用模块就不能访问exports的对象及其属性；

## AMD
- 提前加载
- 加载模块：require([module], function(module){})；

- 定义模块：define([module], function(module){})；module为依赖模块；

- require.js（前端模块化管理的工具库）实现js文件的异步加载，避免网页失去响应；管理模块之间的依赖性，便于代码的编写和维护；require.js是使用创建script元素，通过指定script元素的src属性来实现加载模块的；

- 依赖前置，尽早的执行模依赖块，执行顺序不一定是先1后2；

- 加载非规范的模块
```javascript
 require.config({
    baseUrl: "js/lib",
    paths: {
        "jquery": "jquery.min",
　　  "underscore": "underscore.min",
　　  "backbone": "backbone.min"
　 },
    shim: {
        'underscore':{
            exports: '_'
　　　},
　　　'backbone': {
　　　　deps: ['underscore', 'jquery'],
　　　　exports: 'Backbone'
　　　}
　}
});
// exports值（输出的变量名），表明这个模块外部调用时的名称；deps数组，表明该模块的依赖性
```
## CMD
- 按需加载
- define(function(require, exports, module){
    var a = require('a');
    a.foo();
    };

- Sea.js

- 依赖就近，且在真正需要使用依赖模块时才执行该模块，顺序固定；

- AMD和CMD最大的区别是对依赖模块的执行时机处理不同，而不是加载的时机或者方式不同，二者皆为异步加载模块；

- AMD依赖前置，js可以方便知道依赖模块是谁，立即加载；而CMD就近依赖，需要使用把模块变为字符串解析一遍才知道依赖了那些模块

##  requirejs简单实现
```javascript
  //标记已经加载成功的个数 
var REQ_TOTAL = 0; 
//模块导出 
window.exports = {}; 
//记录各个模块的顺序 
var exp_arr = [];
//判断是否数组 
function isArray(param) { 
  return param instanceof Array; 
} 
//require 真正实现 
function require(arr, callback) { 
  var req_list; 
  if(isArray(arr)) { 
    req_list = arr; 
  } else { 
    req_list = [arr]; 
  } 
  var req_len = req_list.length; 
  //模块逐个加载 
  for(var i=0;i<req_len;i++) { 
    var req_item = req_list[i]; 
    var $script = createScript(req_item, i); 
    var $node = document.querySelector('head'); 
    (function($script) { 
      //检测script 的onload事件 
      $script.onload = function() { 
        REQ_TOTAL++; 
        var script_index = $script.getAttribute('index'); 
        exp_arr[script_index] = exports; 
        window.exports = {}; 
        //所有链接加载成功后，执行callback 
        if(REQ_TOTAL == req_len) { 
          callback && callback.apply(exports, exp_arr); 
        } 
      } 
      $node.appendChild($script); 
    })($script); 
  } 
} 
//创建一个script标签 
function createScript(src, index) { 
  var $script = document.createElement('script'); 
  $script.setAttribute('src', src); 
  $script.setAttribute('index', index); 
  return $script; 
}
```



