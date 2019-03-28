# 浏览器
浏览器使用流式布局模型 (Flow Based Layout)。
浏览器会把HTML解析成DOM，把CSS解析成CSSOM，DOM和CSSOM合并就产生了Render Tree。
有了RenderTree，我们就知道了所有节点的样式，然后计算他们在页面上的大小和位置，最后把节点绘制到页面上。
由于浏览器使用流式布局，对Render Tree的计算通常只需要遍历一次就可以完成，但table及其内部元素除外，他们可能需要多次计算，通常要花3倍于同等元素的时间，这也是为什么要避免使用table布局的原因之一。

## 回流和重绘
**回流必将引起重绘，重绘不一定会引起回流。**
当Render Tree中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为**回流**。
会导致回流的操作：
* 页面首次渲染
* 浏览器窗口大小发生改变
* 元素尺寸或位置发生改变
* 元素内容变化（文字数量或图片大小等等）
* 元素字体大小变化
* 添加或者删除可见的DOM元素
* 激活CSS伪类（例如：:hover）
* 查询某些属性或调用某些方法

一些常用且会导致回流的属性和方法：

clientWidth、clientHeight、clientTop、clientLeft
offsetWidth、offsetHeight、offsetTop、offsetLeft
scrollWidth、scrollHeight、scrollTop、scrollLeft
scrollIntoView()、scrollIntoViewIfNeeded()
getComputedStyle()
getBoundingClientRect()
scrollTo()

当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为**重绘。**

**如何避免**
CSS

避免使用table布局。
尽可能在DOM树的最末端改变class。
避免设置多层内联样式。
将动画效果应用到position属性为absolute或fixed的元素上。
避免使用CSS表达式（例如：calc()）。

JavaScript

避免频繁操作样式，最好一次性重写style属性，或者将样式列表定义为class并一次性更改class属性。
避免频繁操作DOM，创建一个documentFragment，在它上面应用所有DOM操作，最后再把它添加到文档中。
也可以先为元素设置display: none，操作结束后再把它显示出来。因为在display属性为none的元素上进行的DOM操作不会引发回流和重绘。
避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。
对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。

## localStorage和sessionStorage区别
localstorage 重启浏览器后还在
sessionstorage 重启后无

* * *

# VUE
v-if(判断是否隐藏)、v-for(把数据遍历出来)、v-bind(绑定属性)、v-model(实现双向绑定)
## 优点
低耦合性，View和Model可以独立变化，一个ViewModel可以绑定到多个View上
可重用性，把视图逻辑放在ViewModel里，
可以让很多Viw重用
独立开发，专注于业务逻辑和数据的开发（ViewModel）
可测试，针对ViewModel
## MVVM
一种设计思想，model 层代表数据模型，也可以在model中定义数据修改和操作的业务逻辑；view代表UI组件，负责将数据模型转化成UI展现出来；ViewModel是一个同步View和Model的对象。
在MVVM架构下，View和Model不直接联系而通过ViewModel进行交互，Model和ViewModel之间交互双向，View数据变化会同步到Model中，而Model数据变化也立即反应到View上。
ViewModel通过双向数据绑定把View层和Model层连接了起来，双向同步是自动的，由MVVM统一管理。
## 双向绑定的原理！！
vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty()来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

具体步骤： 
第一步：需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter 这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化

第二步：compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

第三步：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要做的事情是:

在自身实例化时往属性订阅器(dep)里面添加自己
自身必须有一个 update()方法
待属性变动 dep.notice()通知时，能调用自身的 update() 方法，并触发 Compile 中绑定的回调，则功成身退。
第四步：MVVM 作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据 model 变更的双向绑定效果。
## 生命周期
**创建前/后：** beforeCreate阶段，vue实例的挂载元素el还没
**载入前/后：** beforeMount阶段，vue实例的$el和data都出实话了，但仍为挂载前为虚拟的dom节点，data.message还未更换。mounted阶段，=vue实例挂载完成，data.message渲染成功
**更新前/后** 当tata变化时会出发onforeUpdata和update方法
**销毁前/后** 在执行destroy方法后，对data的改变不会再出发周期函数，说明此时vue实例已经解除了事件监听和dom帮名，但dom树仍在
## 懒加载（按需加载路由）
webpack 中提供了 require.ensure()来实现按需加载。以前引入路由是通过 import 这样的方式引入，改为 const 定义的方式进行引入。

不进行页面按需加载引入方式：
import  home   from '../../common/home.vue'
进行页面按需加载的引入方式：
const  home = r => require.ensure( [], () => r (require('../../common/home.vue')))
## vuex ！！
##### vuex 有哪几种属性
有 5 种，分别是 state、getter、mutation、action、module
##### vuex 的 store 特性是什么
* vuex 就是一个仓库，仓库里放了很多对象。其中 state 就是数据源存放地，对应于一般 vue 对象里面的 data
* state 里面存放的数据是响应式的，vue 组件从 store 读取数据，若是 store 中的数据发生改变，依赖这相数据的组件也会发生更新
* 它通过 mapState 把全局的 state 和 getters 映射到当前组件的 computed 计算属性
##### vuex 的 getter 特性是什么
* getter 可以对 state 进行计算操作，它就是 store 的计算属性
* 虽然在组件内也可以做计算属性，但是 getters 可以在多给件之间复用
* 如果一个状态只在一个组件内使用，是可以不用 getters

##### vuex 的 mutation 特性是什么

* action 类似于 muation, 不同在于：action 提交的是 mutation,而不是直接变更状态
* action 可以包含任意异步操作

##### vue 中 ajax 请求代码应该写在组件的 methods 中还是 vuex 的 action 中

* 如果请求来的数据不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入 vuex 的 state 里
* 如果被其他地方复用，请将请求放入 action 里，方便复用，并包装成 promise 返回

##### 不用 vuex 会带来什么问题

* 可维护性会下降，你要修改数据，你得维护 3 个地方
* 可读性下降，因为一个组件里的数据，你根本就看不出来是从哪里来的
* 增加耦合，大量的上传派发，会让耦合性大大的增加，本来 Vue 用 Component 就是为了减少耦合，现在这么用，和组件化的初衷相背

##### vuex 原理
vuex 仅仅是作为 vue 的一个插件而存在，不像 Redux,MobX 等库可以应用于所有框架，vuex 只能使用在 vue 上，很大的程度是因为其高度依赖于 vue 的 computed 依赖检测系统以及其插件系统，

vuex 整体思想诞生于 flux,可其的实现方式完完全全的使用了 vue 自身的响应式设计，依赖监听、依赖收集都属于 vue 对对象 Property set get 方法的代理劫持。最后一句话结束 vuex 工作原理，vuex 中的 store 本质就是没有 template 的隐藏着的 vue 组件；

#### 使用 Vuex 只需执行 Vue.use(Vuex)，并在 Vue 的配置中传入一个 store 对象的示例，store 是如何实现注入的？
Vue.use(Vuex) 方法执行的是 install 方法，它实现了 Vue 实例对象的 init 方法封装和注入，使传入的 store 对象被设置到 Vue 上下文环境的$store 中。因此在 Vue Component 任意地方都能够通过 this.$store 访问到该 store。

#### state 内部支持模块配置和模块嵌套，如何实现的？
在 store 构造方法中有 makeLocalContext 方法，所有 module 都会有一个 local context，根据配置时的 path 进行匹配。所以执行如 dispatch('submitOrder', payload)这类 action 时，默认的拿到都是 module 的 local state，如果要访问最外层或者是其他 module 的 state，只能从 rootState 按照 path 路径逐步进行访问。

#### 在执行 dispatch 触发 action(commit 同理)的时候，只需传入(type, payload)，action 执行函数中第一个参数 store 从哪里获取的？
store 初始化时，所有配置的 action 和 mutation 以及 getters 均被封装过。在执行如 dispatch('submitOrder', payload)的时候，actions 中 type 为 submitOrder 的所有处理方法都是被封装后的，其第一个参数为当前的 store 对象，所以能够获取到 { dispatch, commit, state, rootState } 等数据。

#### Vuex 如何区分 state 是外部直接修改，还是通过 mutation 方法修改的？
Vuex 中修改 state 的唯一渠道就是执行 commit('xx', payload) 方法，其底层通过执行 this._withCommit(fn) 设置_committing 标志变量为 true，然后才能修改 state，修改完毕还需要还原_committing 变量。外部修改虽然能够直接修改 state，但是并没有修改_committing 标志位，所以只要 watch 一下 state，state change 时判断是否_committing 值为 true，即可判断修改的合法性。

#### 调试时的"时空穿梭"功能是如何实现的？
devtoolPlugin 中提供了此功能。因为 dev 模式下所有的 state change 都会被记录下来，'时空穿梭' 功能其实就是将当前的 state 替换为记录中某个时刻的 state 状态，利用 store.replaceState(targetState) 方法将执行 this._vm.state = state 实现。


## 组件之间的传值
**孙子组件Login.vue NotFound.vue**
```
<template>html</template>
<script>
    export default = { 这一大括号的就是组件传给爸爸的数据了
        data(){
            return{
                变量
            }
        }，
        methods:{
            一些function 互相调来调去 一些ajax
        }，
        mounted: function () {
            this.getYearsOption(); 加载时自动执行的function
            this.getGourpOption();
        }
    }
</script>
```


**子组件routes.js**
```
import Login from './views/Login.vue'
import NotFound from './views/404.vue'
let routes = [
    {
        path: '/login',
        component: Login,
        name: '',
        hidden: true
    },
    {
        path: '/404',
        component: NotFound,
        name: '',
        hidden: true
    }
]
export default = routes;
```

**父组件main.js**
import routes from './routes'
const router = new VueRouter({routes})
* * *
# CSS
## 盒模型

## 居中
**仅居中元素定宽高适用**
##### absolute（指定div块左上角的绝对位置） + 负margin（左上角偏移）
```
.parent{
  position:relative
}
.child{
  position:absolute;
  top:50%;
  left:50%;
  margin-left:-50px; //-div块宽度的一半
  margin-top:-50px; //-div块高度的一半
}
```
**居中元素不定宽高**
##### absolute + transform：依赖translate属性
```
.parent{
  position:relative
}
.child{
  position:absolute;
  top:50%;
  left:50%;
  transform: translate(-50%, -50%);
}
```

##### css-table： 将div元素按table显示
```
.parent{
   display: table-cell;
   text-align: center;
   vertical-align: middle;
}
.child{
  display: inline-block;
}

```
##### flex： 需要兼容flex
```
.parent{
    display: flex;
    justify-content: center;
    align-items: center;
}
```
## 让高度始终为宽度一半 宽度不指定
让heigth=0，高度又padding-top填充，高度自适应为外层div的10%，宽度自适应为外层div的20%，这样高度一定是宽度的一半，让外层div随窗口自适应blabla
```
.parent {
    width: 100%;
}
.child {
    width: 20%; height: 0;
    padding-top: 10%;
    background: black;
}
```
vw：基于视窗的宽度计算，1vw 等于视窗宽度的百分之一
vh：基于视窗的高度计算，1vh 等于视窗高度的百分之一
```
.parent {
    width: 100%;
}
.child {
    width: 20vw; 
    height: 10vw;
    background: black;
}
```
# 网络
## 从输入URL到页面加载发生了什么

* 1.DNS解析域名
* 2.TCP连接
* 3.发送HTTP请求
* 4.服务器处理请求并返回HTTP报文
* 5.浏览器解析渲染页面

    浏览器是一个边解析边渲染的过程。首先浏览器解析HTML文件构建DOM树，然后解析CSS文件构建渲染树，等到渲染树构建完成后，浏览器开始布局渲染树并将其绘制到屏幕上。这个过程涉及到两个概念: reflow(回流)和repain(重绘)。DOM节点中的各个元素都是以盒模型的形式存在，这些都需要浏览器去计算其位置和大小等，这个过程称为relow;当盒模型的位置,大小以及其他属性，如颜色,字体,等确定下来之后，浏览器便开始绘制内容，这个过程称为repain。页面在首次加载时必然会经历reflow和repain。reflow和repain过程是非常消耗性能的，尤其是在移动设备上，它会破坏用户体验，有时会造成页面卡顿。所以我们应该尽可能少的减少reflow和repain。
    JS的解析是由浏览器中的JS解析引擎完成的。JS是单线程运行，也就是说，在同一个时间内只能做一件事，所有的任务都需要排队，前一个任务结束，后一个任务才能开始。但是又存在某些任务比较耗时，如IO读写等，所以需要一种机制可以先执行排在后面的任务，这就是：同步任务(synchronous)和异步任务(asynchronous)。JS的执行机制就可以看做是一个主线程加上一个任务队列(task queue)。同步任务就是放在主线程上执行的任务，异步任务是放在任务队列中的任务。所有的同步任务在主线程上执行，形成一个执行栈;异步任务有了运行结果就会在任务队列中放置一个事件；脚本运行时先依次运行执行栈，然后会从任务队列里提取事件，运行任务队列中的任务，这个过程是不断重复的，所以又叫做事件循环(Event loop)。
    浏览器在解析过程中，如果遇到请求外部资源时，如图像,iconfont,JS等。浏览器将重复1-6过程下载该资源。请求过程是异步的，并不会影响HTML文档进行加载，但是当文档加载过程中遇到JS文件，HTML文档会挂起渲染过程，不仅要等到文档中JS文件加载完毕还要等待解析执行完毕，才会继续HTML的渲染过程。原因是因为JS有可能修改DOM结构，这就意味着JS执行完成前，后续所有资源的下载是没有必要的，这就是JS阻塞后续资源下载的根本原因。CSS文件的加载不影响JS文件的加载，但是却影响JS文件的执行。JS代码执行前浏览器必须保证CSS文件已经下载并加载完毕。

* 6.连接结束

## CSRF：跨站请求伪造
#### CSRF攻击原理
用户登录A网站
A网站确认身份（给客户端cookie）
B网站页面向A网站发起请求（带上A网站身份：用户是在登录状态下的话，后端就以为是用户在操作）
#### CSRF防御

* Get 请求不对数据进行修改
* 不让第三方网站访问到用户 Cookie
* 阻止第三方网站请求接口： 检查Referer字段
HTTP头中有一个Referer字段，这个字段用以标明请求来源于哪个地址。在处理敏感数据请求时，通常来说，Referer字段应和请求的地址位于同一域名下。以银行操作为例，Referer字段地址通常应该是转账按钮所在的网页地址，应该也位于已知的地址www.examplebank.com之下。而如果是CSRF攻击传来的请求，Referer字段会是包含恶意网址的地址，不会位于www.examplebank.com之下，这时候服务器就能识别出恶意的访问。
这种办法简单易行，工作量低，仅需要在关键访问处增加一步校验。但这种办法也有其局限性，因其完全依赖浏览器发送正确的Referer字段。虽然http协议对此字段的内容有明确的规定，但并无法保证来访的浏览器的具体实现，亦无法保证浏览器没有安全漏洞影响到此字段。并且也存在攻击者攻击某些浏览器，篡改其Referer字段的可能。

* 请求时附带验证信息，比如验证码或者 Token
由于CSRF的本质在于攻击者欺骗用户去访问自己设置的地址，所以如果要求在访问敏感数据请求时，要求用户浏览器提供不保存在cookie中（因为cookie自动添加到请求报头），并且攻击者无法伪造的数据作为校验，那么攻击者就无法再执行CSRF攻击。这种数据通常是表单中的一个数据项。服务器将其生成并附加在表单中，其内容是一个伪乱数。当客户端通过表单提交请求时，这个伪乱数也一并提交上去以供校验。正常的访问时，客户端浏览器能够正确得到并传回这个伪乱数，而通过CSRF传来的欺骗性攻击中，攻击者无从事先得知这个伪乱数的值，服务器端就会因为校验token的值为空或者错误，拒绝这个可疑请求。


## XSS：跨站脚本攻击
XSS ( Cross Site Scripting ) 是指恶意攻击者利用网站没有对用户提交数据进行转义处理或者过滤不足的缺点，进而添加一些代码，嵌入到web页面中去。使别的用户访问都会执行相应的嵌入代码。从而盗取用户资料、利用用户身份进行某种动作或者对访问者进行病毒侵害的一种攻击方式。
#### XSS攻击注入点

* HTML节点内容

    如果一个节点内容是动态生成的，而这个内容中包含用户输入。

* HTML属性

    某些节点属性值是由用户输入的内容生成的。那么可能会被封闭标签后添加script标签。
    <img src="${image}"/>
    <img src="1" onerror="alert(1)" />

* Javascript代码

    JS中包含由后台注入的变量或用户输入的信息。
    var data = "#{data}";
    var data = "hello"; alert(1);"";
#### XSS防御

* 转义字符（！）

* CSP 内容安全策略（！）
CSP 本质上就是建立白名单，开发者配置规则明确告诉浏览器哪些外部资源可以加载和执行。


## 跨域问题
[同源&跨域](https://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)
对于完全不同源的网站，目前有三种方法，可以解决跨域窗口的通信问题。
* 片段识别符（fragment identifier）
* window.name
* 跨文档通信API（Cross-document messaging）

同源政策规定，AJAX请求只能发给同源的网址，否则就报错。
除了架设服务器代理（浏览器请求同源服务器，再由后者请求外部服务），有三种方法规避这个限制。

* JSONP
* WebSocket
* CORS

**jsonp**
浏览器上虽然有同源限制，但是像 srcipt标签、link标签、img标签、iframe标签，这种在标签上通过src地址来加载一些内容的时候浏览器是允许进行跨域请求的。

所以JSONP的原理就是：

* 创建一个script标签，这个script标签的src就是请求的地址；
* 这个script标签插入到DOM中，浏览器就根据src地址访问服务器资源
* 返回的资源是一个文本，但是因为是在script标签中，浏览器会执行它
* 而这个文本恰好是函数调用的形式，即函数名（数据），浏览器会把它当作JS代码来执行即调用这个函数
* 只要提前约定好这个函数名，并且这个函数存在于window对象中，就可以把数据传递给处理函数。

```
$.ajax({  
    type : "get",  
    url : "跨域地址",  
    dataType : "jsonp",//数据类型为jsonp  
    jsonp: "callback",//服务端用于接收callback调用的function名的参数【后台接受什么参数，我们就传什么参数】我们上面设置是callback
    success : function(data){  
        //结果处理
    },  
    error:function(data){  
          console.log(data);
    }  
});
```
* * *

# 项目
## 后端鉴权
#### session：
保存在服务器上，一个站点可能有多台服务器，无法知道用户注册的session保存在哪一台服务器上
session 有如用户信息档案表, 里面包含了用户的认证信息和登录状态等信息.
可以理解为一个状态列表，拥有一个唯一识别符号sessionId，通常存放于cookie中。
服务器收到cookie后解析出sessionId，再去session列表中查找，才能找到相应session。
#### cookie：
首先，客户端会发送一个http请求到服务器端。服务器端接受客户端请求后，建立一个session，并发送一个http响应到客户端，这个响应头，其中就包含Set-Cookie头部。该头部包含了sessionId。Set-Cookie格式如下，Set-Cookie: value[; expires=date][; domain=domain][; path=path][; secure]
在客户端发起的第二次请求，假如服务器给了set-Cookie，浏览器会自动在请求头中添加cookie服务器接收请求，分解cookie，验证信息，核对成功后返回response给客户端![4cc2504c227b8168198bf2efa036e0d5.png](evernotecid://2DDF986E-4FEC-4176-9215-65E1257FD0DF/appyinxiangcom/1651529/ENResource/p189)
**注意**

* cookie只是实现session的其中一种方案。虽然是最常用的，但并不是唯一的方法。禁用cookie后还有其他方法存储，比如放在url中
* 现在大多都是Session + Cookie，但是只用session不用cookie，或是只用cookie，不用session在理论上都可以保持会话状态。可是实际中因为多种原因，一般不会单独使用
* 用session只需要在客户端保存一个id，实际上大量数据都是保存在服务端。如果全部用cookie，数据量大的时候客户端是没有那么多空间的。
* 如果只用cookie不用session，那么账户信息全部保存在客户端，一旦被劫持，全部信息都会泄露。并且客户端数据量变大，网络传输的数据量也会变大

#### token
##### token 的认证流程
与cookie很相似
用户登录，成功后服务器返回Token给客户端。
客户端收到数据后保存在客户端
客户端再次访问服务器，将token放入headers中
服务器端采用filter过滤器校验。校验成功则返回请求数据，校验失败则返回错误码
##### token 存放
token在客户端一般存放于localStorage，cookie，或sessionStorage中，在服务器一般存于数据库中
##### 组成
uid: 用户唯一身份标识
time: 当前时间的时间戳
sign: 签名, 使用 hash/encrypt 压缩成定长的十六进制字符串，以防止第三方恶意拼接
固定参数(可选): 将一些常用的固定参数加入到 token 中是为了避免重复查库


## ReqIF
import xml->parser->CSVinstance->map template->view
优化：
封装函数
减少请求次数 
数据结构优化 减少遍历的算法复杂度





## express（node.js）
## computed、watch、data 的各自适用范围（vue）
## webpack流程、原理、原则（webpack）

## http2
HTTP2 的优势:
1、二进制分帧层 (Binary Framing Layer)
2、多路复用 (MultiPlexing)
3、服务端推送 (Server Push)
4、Header 压缩 (HPACK)
5、应用层的重置连接
6、请求优先级设置
7、流量控制
## 订阅-发布模式


TODO：
ReqIF 优化 parser template
es6 es7 ssynchronize promise（宏观队列 微观队列 事件循环） async 
webpack



js h5 css vue react/redux 网络 浏览器 webpack node.js（IO/express） grafphql/restfulAPI 


简历
挑一段实习经历介绍
有遇到什么难点
怎么解决的
❎这样不会有什么问题嘛

js
❎有什么方法检测类型
❎typeof a 返回字符串“object”
typeof Array 返回“fucntion” 
创建一个类 创建这个类的实例test typeof test返回“object”而不是自定义类名
test.__proto__
{constructor: ƒ}
test instanceof 类名/object 都返回true

❎为什么返回一个字符串
❎js的原型链

❎ajax 状态码0189i 哦人234
重排和重绘 如何避免

css
可以让元素看不见的方法
❎display：none 和visibility：hidden的区别
position的取值有哪些
❎position absolute和relative的原理
模态框的居中
怎样让一个dom铺满
window。reload resize

os
❎进程之间的通信

ds
栈和队列的区别
希尔排序的原理
❎平衡二叉树

网络
掩码的作用
32位地址 30位掩码 有多少个可用IP
ipv4和ipv6的区别

