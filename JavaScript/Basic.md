## 标签含义
- async
    Indicates that the script should begin downloading immediately but should not prevent other actions on the page such as downloading resources or waiting for other scripts to load. Valid only for external script ﬁles.
- charset
    The character set of the code speciﬁed using the src attribute. This attribute is rarely used, because most browsers don’t honor its. value.
- src
    Indicates an external ﬁ le that contains code to be executed.
- type
    Replaces language; indicates the content type (also called MIME type) of the scripting language being used by the code block. Traditionally, this value has always been “text/javascript”, though both “text/javascript” and “text/ecmascript” are deprecated. 


同时有src以及内部代码，内部代码会被忽略

用async标签的时候，注意两个js文件之间没有依赖


## 尽量用src外部文件
- 可以有一个较好的目录
- 浏览器可以设置缓存，如果多个页面共用一个js文件，可以缓存




如果不用var，就是全局的变量

```js
var message = “hi”, found = false, age = 29;
```

### primitive types：
- Undeﬁned
- Null
- Boolean
- Number
- String

### typeof
- undefined
- boolean
- string
- number
- object
- function

```js
>>   //无符号右移
>>>  //有符号右移
== & ===
```

```js
function howManyArgs() { alert(arguments.length); }

howManyArgs(“string”, 45);  //2
howManyArgs();              //0
howManyArgs(12);            //1
```

```js
function doAdd() { 
    if(arguments.length == 1) { 
        alert(arguments[0] + 10); 
        
    } else if (arguments.length == 2) { 
        alert(arguments[0] + arguments[1]); 
            
    } 
}

doAdd(10);                  //20
doAdd(30, 20);              //50
```

### reference value copy

```js
var obj1 = new Object(); 
var obj2 = obj1; 
obj1.name = “Nicholas”; 
alert(obj2.name); //”Nicholas”

function setName(obj) {
    obj.name = "test";
}
var person = new Object();
setName(person);
alert(person.name);//"test"
```

![Screen Shot 2018-11-29 at 9.47.58 AM.png](resources/1F43E132EBD6B232C3A87D1E1689B5AA.png =724x492)
引用类型变量复制的时候，都指向同一个对象

### 延长作用域链
`with` or `try catch`

### JS的垃圾回收机制

基本用的标记清楚的回收策略（反转）
可以通过手工设为null来解除变量的引用，以让变量脱离环境从而触发GC

### JS数组
栈：push()  pop()
队列：push() shift()

排序：sort() reverse()

concat()创建新的数组
splice()删除插入和替换

indexOf()
lastIndexOf()

迭代方法
- every()
- filter()
- forEach()
- map()
- some()