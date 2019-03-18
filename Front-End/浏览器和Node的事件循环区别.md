https://juejin.im/post/5c337ae06fb9a049bc4cd218#heading-12

浏览器线程
- GUI渲染线程
- JS引擎线程
- 定时触发器线程
- 事件触发线程
- 异步http线程


GUI线程和JS线程互斥
JS执行时GUI会挂起，任务队列空闲时，主线程才会执行GUI


浏览器的Event Loop
![IMAGE](resources/1924325916E018A316E1F79008368EB6.jpg =800x613)
macro宏任务，micro微任务
宏任务有多个：settimeout，script，I/O

微任务只有一个：Promise，MutationObserver


执行一个macro-task
-> 执行多个micro-task
-> 渲染更新
-> 处理worker


Node：
timers(setTimeout回调)->
I/O callback阶段(处理上一轮循环中未执行的I/O回调)->
idel，prepare阶段：仅node内部使用->
poll阶段：轮询，获取新的I/O事件->
check阶段：执行setImmediate()的回调->
close callbacks阶段：执行socket的close事件回调->

