## BOM

Browser Object Model 浏览器对象模型， 操作浏览器窗口的API

具体包括：window、history、location、navigator、dom、event、screen

---

#### window对象

浏览器窗口相关

- window为浏览器实例，同时也是JS环境下的全局对象

窗口大小相关API

- 获取窗口宽高：window.clientWidth、window.clientHeight
- 调整窗口宽高到：window.resizeTo(x,y)

导航和打开窗口

- window.open("url", "窗口名")：窗口名存在几个特殊值
  - _self：在当前窗口打开，可后退
  - _blank：在新的窗口打开，可打开多个
- window.close()：关闭窗口

系统对话框

- alert()：显示带有一段消息和一个确认按钮的警告框
- confirm()：显示带有一段消息及确认和取消按钮的对话框
- prompt()：显示可提示用户输入的对话框

#### location对象

载入窗口的URL相关

| 属性名   | 例子                          | 说明                                         |
| -------- | ----------------------------- | -------------------------------------------- |
| hash     | \#content                     | 返回URL中的hash\(\#后字符\)                  |
| host     | www\.wrox\.com:80             | 返回服务器名称和端口号                       |
| hostname | www\.wrox\.com                | 返回不带端口号的服务器名称                   |
| href     | http://www\.wrox\.com:80      | 返回当前加载页面完整的URL                    |
| pathname | /home                         | 返回URL中的目录和文件名                      |
| port     | 8080                          | 返回URL中指定的端口号                        |
| protocol | http:                         | 返回页面使用的协议  通常是http:或https:      |
| seacrh   | ?q=js                         | 返回URL查询字段\(?后字符\)                   |
| assign   | assign("http:www.baidu.com")  | 同herf，加载新页面                           |
| replace  | replace("http:www.baidu.com") | 同assign()，但新地址会在历史记录中替换旧地址 |

获取URL查询参数

```javascript
function getQueryStringArgs(){
  // 字符串并去掉开头的问号
  let qs = (location.search.length > 0 ? location.search.substring(1) : ""),
      args = {},
      items = qs.spilt("&"):[],
      item = null, name = null, value = null
  for(let i=0; i<items.length; i++){
    item = items[i].split("=")
    name = item[0]
    value = item[1]
    // name不为空，则存入args
    if(name.length){
      args[name] = value
    }
  }
  return args
}
```

#### navigator对象

当前Web浏览器相关

- 获取官方浏览器的名称字符串：navigator.appName
- 获取用户代理头的字符串：navigator.userAgent
- 获取用户操作系统的默认语言：navigator.userLanguage
- 获取当前是否启用cookie：navigator.cookieEnabled

#### screen对象

用户屏幕信息相关

- 获取用户设备屏幕的宽高(px)：screen.width/height
- 获取窗口可以使用的屏幕的宽高：screen.availWidth/availHeight
- 填充用户的屏幕：window.resizeTo(screen.availWidth, screen.availHeight)

#### history对象

用户上网的历史记录相关

- 后退一页：history.go(-1)	简写：history.back()
- 前进一页：history.go(1)     简写：history.forward()
- 获取用户打开的第一个页面：history.length === 0