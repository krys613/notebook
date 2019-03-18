## `let`只在代码块内部有效

```js
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1


for (let i = 0; i < 10; i++) {
  // ...
}

console.log(i);
// ReferenceError: i is not defined





var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10

var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6



for循环中，循环变量是一个父作用域，函数体内部是一个子作用域

for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

## `let`比`var`更严谨
用`let`的话，只有在申明后才能使用

```js
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

锁区。`let`和`const`都会在其代码块形成暂时性死区(temporal dead zone)，在申明之前调用都会发生错误。
> TDZ的本质是，一旦进入某个作用域，虽然所需的变量已经存在，但要申明后才能获取和使用

同时`typeof`运算符就不是安全的了

```js
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}


if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}


typeof x; // ReferenceError
let x;
typeof undeclared_variable // "undefined"
```

不能重复申明

```js
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}

function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}
```

## 块级作用域

es6有了块级作用域
因为不同浏览器的原因，应该避免在块级作用域里面申明函数，有需要的话写成函数表达式

```js
// 函数声明语句
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

## `const`
`const`是只读的变量，申明时就应该赋值，否则会报错，同时不能重复声明

```js
const foo;
// SyntaxError: Missing initializer in const declaration

var message = "Hello!";
let age = 25;

// 以下两行都会报错
const message = "Goodbye!";
const age = 30;
```

`const`对于简单的类型(数值、字符串、布尔)可以保证benign改动，但对于复杂的类型(对象/数组)是不能保证不固定的

```js
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only

const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错
```

如果要保证对象不可变，用`Object.freeze`方法

```js
const foo = Object.freeze({});

// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;



```

这是将对象以及其内部属性全冻结的函数

```js
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

## global对象

```js
// CommonJS 的写法
require('system.global/shim')();

// ES6 模块的写法
import shim from 'system.global/shim'; shim();





// CommonJS 的写法
var global = require('system.global')();

// ES6 模块的写法
import getGlobal from 'system.global';
const global = getGlobal();
```