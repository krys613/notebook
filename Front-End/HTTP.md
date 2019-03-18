### 请求
GET POST PUT DELETE CONNECT

### POST方式
- application/x-www-form-urlencoded （ajax， utf-8）
- multipart/form-data（上传文件）
- application/json
- text/xml

### GET和POST
- GET请求的数据会附在URL之后，http协议头上？分割URL和数据，参数用&连接
- POST放在包体中

- 浏览器对url长度的限制
- 服务器对post大小的限制

post比get安全


soap：xml消息的post版本

### 状态码
200成功
204请求成功但没有资源返回
301永久重定向 http到https
302临时跳转   404到首页，登录
304原来的缓存可以用
400请求错误
401未授权，需要验证
403请求资源的访问被拒绝
404不存在
500内部错误
503服务器出错