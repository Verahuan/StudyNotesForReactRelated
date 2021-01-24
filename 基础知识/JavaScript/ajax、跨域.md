### Ajax

Asynchronous Js + XML，核心是XMLHttpRequest对象（XHR）

主要作用：在不刷新页面的情况下异步发送请求并接受响应，最后局部更新页面

核心技术为XHR对象，该对象可发起http请求，我们可以监听器readystate的变化获得响应

#### XHR创建

- 创建XHR对象
  - var xhr = new XMLHttpRequest();
  - 兼容IE6以下，ActiveXObject('Microsoft.XMLHTTP')；

#### XHR用法

##### 发送请求

```javascript
xhr.open("get", "example.txt", true)
xhr.send(null)
```

- **xhr.open(arg1, arg2, arg3)**
  - arg1：要发送的请求类型，get或者post等
  - arg2：请求的URL
  - arg3：是否异步发送请求，true异步
  - URL相对于执行代码的当前页面，也可以是绝对路径
  - open()方法并不会真正发送请求，而是启动一个请求以备发送
- **xhr.send(arg)**
  - arg：作为请求主体发送的数据，如果不需要通过请求主体发送数据则设为null

##### 接收响应

在收到响应后，响应的数据会自动填充XHR对象属性

- **responseText**：作为响应主体被返回的文本
- **responseXML**：如果响应的内容类型是“text/xml”或“application/xml”，这个响应保存包含着响应数据的XML DOM文档
  - 无论什么响应内容，都会保存到responseText，对于非XML数据，responseXML属性值为null
- **status**：响应的HTTP状态
  - 2xx：表示请求处理成功，200
  - 3xx：需要重定向，浏览器直接处理，304
  - 4xx：客户端请求错误，404
  - 5xx：服务器端错误，502
- **statusText**：HTTP状态说明

**readyState**：该属性表示请求、响应过程的当前活动阶段

- 0：未初始化Uninitialized，尚未调用open()
- 1：启动Open，已经open()，但未send()
- 2：发送Sent，已经send()，但尚未收到响应
- 3：接收Receiving，已经收到部分响应数据
- 4：完成Loaded，已经收到全部响应数据，并可使用
- readyState值每次更改都会触发onreadystatechange事件

一般顺序为：

- 创建xhr对象
- 设置onreadystatechange事件处理程序
- xhr.open()
- xhr.send(null)

在接收到响应之前，可以通过xhr.abort()方法取消异步请求

- 调用该方法后，XHR对象会停止触发事件，而且也不再允许访问任何与响应有关的对象属性

#### 手写原生ajax

```javascript
var xhr =  window.XMLHttpRequest? 
    new XMLHttpRequest(): 
		new ActiveXObject("Microsoft.XMLHTTP")

xhr.onreadyStatechange = function(){
  if(xhr.readyState === 4){
    if(xhr.status === 200){
      callback(xhr.responseText)
    }
  }
}

//get
xhr.open('get', 'url?key&value', true)
xhr.send(null)

//post
xhr.open('post', 'url', true)
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencodeed")
xhr.send(data)
```

#### 谈谈get、post

- get常用来获取数据，post常用来发送数据
- get会缓存会保存在浏览器记录中，post不会
- get请求参数在url中，post请求参数在请求体中
- get请求有长度及大小限制(由浏览器及服务器决定，常见2k)，post理论上没有限制
- get和post都不安全，毕竟http是明文传输

**它们的本质都是 TCP 链接，并无区别。但是由于 HTTP 的规定以及浏览器/服务器的限制，导致它们在应用过程中可能会有所不同**

#### HTTP头部信息

每个HTTP请求和响应都会带有相应的头部信息，默认情况下，发送XHR请求的同时，还会发送下列头部信息

- Accept：浏览器能够处理的内容类型
- Accept-Charset：浏览器能够显示的字符集
- Accept-Encoding：浏览器能够处理的压缩编码
- Accept-Language：浏览器当前设置的语言
- Connection：浏览器与服务器之间连接的类型
- Cookie：当前页面设置的任何Cookie
- Host：发出请求的页面所在的域
- Referer：发出请求的页面的URI
- User-Agent：浏览器的用户代理字符串

#### 头部方法

xhr.setRequestHeader(header, value)可以设置自定义的请求头部信息

- header：头部字段的名称
- value：头部字段的值
- 该方法需在xhr.open()之后、xhr.send()前设置

xhr.getResponseHeader()：传入头部字段名称，可以取得相应的响应头部信息

xhr.getAllResponseHeaders()：获取包含所有头部信息的长字符串

#### 超时设定

- xhr.timeout：请求在等待响应多少毫秒后就会自动终止
- xhr.ontimrout：终止时会调用ontimeout事件处理程序

#### 进度事件

- loadstart：在接收到响应数据期间持续不断地触发
- progress：在接收响应期间持续不断地触发
- error：在请求发生错误时触发
- abort：在调用abort()而终止连接时触发
- load：在接收到完整的响应数据时触发
- loadend：在通信完成或者触发error、abort、load事件后触发

每个请求都以触发loadstart开始，接下来是一个或多个progross事件，然后触发error、abort、load事件其中一个，最后触发loadend结束

---

### 跨域、同源策略

由于同源策略限制，ajax无法跨域（不一定是浏览器限制发起跨域请求，也有可能是跨域请求可以发起，但是返回结果被浏览器拦截）

- 同源策略限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制
- 同源策略：域名，协议，端口均相同

`http://www.baidu.com/8080/index.html`

| http://       | 协议不同         |
| ------------- | ---------------- |
| www           | 子域名不同       |
| baidu.com     | 主域名不同       |
| 8080          | 端口号不同       |
| www.baidu.com | ip地址和网址不同 |

#### CORS

利用额外的HTTP头来告诉浏览器，让运行在一个Origin(domain) 上的Web应用被准许访问来自不同源服务器上的指定的资源

[阮一峰CORS详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)

CORS请求分为两类：简单请求、复杂请求

##### 简单请求

同时满足以下两大条件

- 方法：
  - HEAD
  - GET
  - POST
- HTTP头部信息不超出以下几种字段
  - Accept
  - Accept-Language
  - Content-Language
  - Last-Event-ID
  - Content-Type：仅限三个值
    - application/x-www-form-urlencoded
    - multipart/form-data
    - text/plain

对于简单请求，浏览器直接发出CORS请求（在请求头加入origin字段），如果Origin指定的域名不在许可范围内，服务器会返回正常的HTTP回应，然后浏览器发现这个回应的头信息没有包含`Access-Control-Allow-Origin`字段，就会报错（XHR.onerror捕获）

如果Origin指定的域名在许可范围，服务器返回的响应对多出如下几个信息字段

```javascript
Access-Control-Allow-Origin: `${Origin}`
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```

- **Access-Control-Allow-Origin**：必须字段，可以指定为*，表示接受任意域名请求
- **Access-Control-Allow-Credentials**：可选字段，表示是否允许发送Cookie，默认为false
- **Access-Control-Expose-Headers**：可选字段，CORS请求时，XHR对象的getResponseHeader()默认只能拿到6个基本字段（Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma），要想拿到其他字段需要该属性来指定

**withCredentials 属性**

由于CORS请求默认不发送Cookie、HTTP认证、SSL证明等，因此要发送cookie分为两步

- 服务器端：`Access-Control-Allow-Credentials: true`
- ajax打开withCredentials：`xhr.withCredentials = true`

需要注意的是，如果要发送Cookie，`Access-Control-Allow-Origin`就不能设为星号，必须指定明确的、与请求网页一致的域名

同时，Cookie依然遵循同源政策，只有用服务器域名设置的Cookie才会上传，其他域名的Cookie并不会上传，且（跨源）原网页代码中的`document.cookie`也无法读取服务器域名下的Cookie

##### 非简单请求

非简单请求是那种对服务器有特殊要求的请求，比如请求方法是`PUT`或`DELETE`，或者`Content-Type`字段的类型是`application/json`

###### 预检请求

非简单请求的CORS请求前，会增加一次"预检"请求（preflight request）

- 预检请求为OPTIONS方法，表示这个请求是用来询问的
- 预检请求包含以下头部信息
  - **Origin**：与简单请求相同
  - **Access-Control-Request-Method**：必须，指定CORS要使用的方法
  - **Access-Control-Request-Headers**：可选，指定CORS要发送的额外头部信息

服务器收到预检请求后，检查以上三个字段后，确认允许跨域请求，就会做出回应，包含上述三个关键字段加**可选字段Access-Control-Max-Age**（将这个预检请求缓存多次时间 /s）

如果服务器否认了预检请求，会返回一个正常的HTTP回应，但是没有任何CORS相关字段。这时候会触发错误，被XMLHTTPRequest对象的onerror回调函数捕获

```javascript
// CORS后台设置  复杂请求（简单请求只有Origin必须，Credentials、Headers可选）

//指定允许接收的域名 *代表全部  必须
//指定允许接收的方法  必须
//指定允许接收的请求头
res.setHeader('Access-Control-Allow-Origin', *);
res.setHeader('Access-Control-Allow-Methods', 'GET,PUT,POST');
res.setHeader('Access-Control-Allow-Headers', 'Content-Type');
//允许Cookie包含在请求中，不允许的话删除该字段
//如果允许发送cookie，Origin就不能设为星号，必须指定明确的、与请求网页一致的域名
//同时Ajax中将withCredentials=true
res.setHeader('Access-Control-Allow-Credentials', 'true');
// 预检的存活时间
res.setHeader('Access-Control-Max-Age', 6)
// 允许返回的头
res.setHeader('Access-Control-Expose-Headers', 'name')
```



---

#### JSONP

jsonp：json with padding，通过script标签不受通同源策略限制来实现跨域（仅限get请求）

- 可以跨域的三个标签：link、img、script

JSONP步骤

- 请求阶段：浏览器创建一个 script 标签，并给其src 赋值(类似 `http://example.com/api/?callback=jsonpCallback`）
- 发送请求：当给script的src赋值时，浏览器就会发起一个请求
- 数据响应：服务端将要返回的数据作为参数和函数名称拼接在一起(格式类似”jsonpCallback({name: 'abc'})”)返回
- 当浏览器接收到了响应数据，由于发起请求的是 script，所以相当于直接调用 jsonpCallback 方法，并且传入了一个参数

JSONP不足

- JSONP从其他域加载执行的，如果其他域不安全，可能会在响应中夹带一些恶意代码
- 确定JSONP请求是否失败不容易，目前通过计时器检测指定时间是否收到

##### 手写jsonp

前端部分

```javascript
// 封装jsonp
function jsonp(req){
  let script = document.createElement("script")
  let url = req.url + '?callback=' + req.callback
  script.src = url
  document.getElementByTagName['head'][0].appendChild(script)
}

// 自定义回调函数
function hello(res){
  alert('hello' + res.data)
}

jsonp({url:"", callback:"hello"})
```

复杂版：[http://www.conardli.top/docs/JavaScript/%E6%89%8B%E5%8A%A8%E5%AE%9E%E7%8E%B0JSONP.html](http://www.conardli.top/docs/JavaScript/手动实现JSONP.html)

后端部分

```javascript
const http = require('http')
const urllib = require('url')

const port = 3000
const data = {'data': 'world'}

http.createServer(function (req, res)){
	const {pathname: url, query} = urllib.parse(req.url, true);
	if(query.callback){
    // 解析出来如果带callback，则调用回调函数，将数据传进去
    let str = `${query.callback(${JSON.stringfy(data)})}`
    res.end(str)
  }else res.end()
}).listen(port, function(){
  console.log('jsonp server is on')
})
```

#### CORS与JSONP对比

- CORS支持所有请求，JSONP只支持GET请求
- CORS可以通过XHR对象来发起请求和获取数据，更好处理错误
- JSONP的优势在于支持老式浏览器，及可以向不支持CORS的网站请求数据

#### document.domain

相同主域名，不同子域名的页面，可以设置document.domain让它们同域

- 默认情况下，document.domain存放的是载入文档的服务器的主机名，可以手动设置这个属性
- 不过是有限制的，只能设置成当前域名或者上级的域名，并且必须要包含一个.号，也就是说不能直接设置成顶级域名。例如：id.qq.com，可以设置成qq.com，但是不能设置成com

限制：同域document提供的是页面间的互操作，需要载入iframe页面

##### 相关面试题

实现A网站登录，B、C网站也可以直接登录：cookie，设置domain为三个网页的顶级域名

如果是a.com、b.com、c.com：单点登录SSO

**单点登录SSO**

sso需要一个独立的认证中心，只有认证中心能接受用户的用户名密码等安全信息，其他系统不提供登录入口，只接受认证中心的间接授权

间接授权通过令牌实现，sso认证中心验证用户的用户名密码没问题，创建授权令牌，在接下来的跳转过程中，授权令牌作为参数发送给各个子系统，子系统拿到令牌，即得到了授权，可以借此创建局部会话，局部会话登录方式与单系统的登录方式相同

#### Nginx反向代理

通过location实现API转发代理

```javascript
server {
 listen 80;
 server_name 127.0.0.1;
 
 location / {
 proxy_pass http://127.0.0.1:3000;
 }
 
 location ~ /api/ {
 proxy_pass http://172.30.1.123:8081;
 }
}
// 监听80端口（Nginx默认启动了80端口），将http://127.0.0.1的所有请求服务转发到127.0.0.1端口为3000；
// 将http://127.0.0.1/api/或者http://127.0.0.1/api/getList请求转发到http://172.30.1.123:8081
```

原理类似Node中间件代理（两次跨域），同源策略只是浏览器需要遵守的标准，但是服务器向服务器发送请求就无需遵循同源策略

![img](https://image.fundebug.com/2019-01-26-03.png)

#### WebSocket

Websocket是HTML5的一个持久化的协议，它实现了浏览器与服务器的全双工通信，本质上是一个基于 TCP 的协议，WebSocket为自定义协议，未加密的连接是wss://，且需要支持这种协议的专门服务器才能工作

建立一个 WebSocket 连接，客户端浏览器首先要向服务器发起一个 HTTP 请求，这个请求和通常的 HTTP 请求不同，包含了一些附加头信息，其中附加头信息"Upgrade: WebSocket"表明这是一个申请协议升级的 HTTP 请求，服务器端解析这些附加的头信息然后产生应答信息返回给客户端，客户端和服务器端的 WebSocket 连接就建立起来了

**WebSocket 是一种双向通信协议，在建立连接之后，WebSocket 的 server 与 client 都能主动向对方发送或接收数据**。同时，WebSocket 在建立连接时需要借助 HTTP 协议，连接建立好了之后 client 与 server 之间的双向通信就与 HTTP 无关了

WebSocket API：

- 实例化一个WebSocket对象并传入要连接的URL，必须是绝对URL
- WebSocket状态：
  - WebSocket.opening(0)：正在建立连接
  - WebSocket.open(1)：已经建立连接
  - WebSocket.closing(2)：正在关闭连接
  - WebSocket.close(3)：已经关闭连接
- 要关闭WebSocket连接的话，通过实例调用close()即可

发送和接收数据

- WebSocket打开后，即可通过连接发送和接收数据；使用send()像服务器发送数据，只能发送纯文本数据即字符串；当服务器向客服端发来消息时，WebSocket对象就会触发message事件，返回数据保存在event.data中

其他事件

- WebSocket还有三个事件，在连接生命周期的不同阶段触发
- open：在成功建立连接时触发
- error：在发生错误时触发，连接不能持续
- close：在关闭连接时触发

```javascript
// socket.html
<script>
    let socket = new WebSocket('ws://localhost:3000');
    socket.onopen = function () {
      socket.send('我爱你');//向服务器发送数据
    }
    socket.onmessage = function (e) {
      console.log(e.data);//接收服务器返回的数据
    }
</script>
// server.js
let express = require('express');
let app = express();
let WebSocket = require('ws');//记得安装ws
let wss = new WebSocket.Server({port:3000});
wss.on('connection',function(ws) {
  ws.on('message', function (data) {
    console.log(data);
    ws.send('我不爱你')
  });
})
```

