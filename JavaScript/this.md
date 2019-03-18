### basic
代表当前函数执行的主体
this在函数被调用时发生绑定
- 函数前是否有.有的话就是谁。不然就是window
- 自执行函数中this永远是window
- 给元素某一个事件绑定方法，this就是当前元素
- 构造函数是当前类的实例
- call、apply、bind


### call，apply，bind
#### 相似
- 都是用来改变this对象的指向
- 第一个参数都是this要指向的对象
- 都可以利用后续参数传参

#### 不同
- call和apply是直接调用，执行
    call参数是一一对应的，apply的参数是一个数组
- bind不是直接调用，而是返回一个改变了的函数。它和和call一样的传参方式

#### 另外
call，apply调箭头函数是，不需要this，第一个参数会被忽略

### 自己写一个bind函数

```js
//bind是创建一个新的函数，改变指向

Function.prototype.myBind = function(thisArg) {
  if (typeof this !== 'function') {
    return;
  }
  var _self = this;
  var args = Array.prototype.slice.call(arguments, 1) //从第二个参数截取
  return function() {
    return _self.apply(thisArg, args.concat(Array.prototype.slice.call(arguments))); // 注意参数的处理
  }
}
```