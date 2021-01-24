## DOM

document object model，文档对象模型，操作HTML和XML的API

---

#### Node

nodeName：节点的属性名

nodeValue：节点的属性值

nodeType：节点的类型

| nodeType值 | 描述                 | nodeName            | nodeValue |
| ---------- | -------------------- | ------------------- | --------- |
| 1          | 元素节点<p> <div>    | 元素标签名          | null      |
| 3          | 文本节点             | #text               | 文本内容  |
| 8          | 注释节点             | #comment            | 注释内容  |
| 9          | document节点         | #document           | null      |
| 11         | DocumentFragment节点 | \#document-fragment | null      |

---

### 常用API

#### 节点创建API

- document.createElement(tagName)：创建元素，创建完需要通过appendChild或insertBefore等方法添加到HTML文档树中
- document.createTextNode(data)：创建一个文本节点，同样需要调用上述方法将其加入HTML文档树中
- document.cloneNode(deep)：克隆节点，deep可选，若为true则节点后代所有节点都会被克隆，否则只克隆自身
  - 克隆完同样需要加入到HTML文档树中
  - 如果复制的元素有id，则复制完后必须修改id
  - 如果通过内联方式绑定的事件克隆后仍在，如果通过addEventListener或onclick绑定的则不在
- document.createDocumentFragment(): 创建一个文档片段，文档片段在内存中，操作其中DOM不会引起回流，因此可以先创建一个文档片段，在其中将DOM生成完整后一次性插入页面

#### 页面修改型API

- parent.appendChild(child)：将指定的child节点添加到parent节点最后一个子节点
- parentNode.insertBefore(newNode,refNode)：将newNode添加到parent的子节点refNode之前
- parent.removeChild(node)：删除子节点node并返回，删除后仍处于内存中
- parent.replaceChild(newChild,oldChild)：`newChild`是替换的节点，可以是新的节点，也可以是页面上的节点，如果是页面上的节点，则其将被转移到新的位置
- 如果新增的为已存在的节点，则原来位置的节点会被移除，若移除节点，节点本身绑定的事件不会消失会一直保存

#### 节点查询型API

- document.getElementById(id)：id唯一，且大小写敏感
- document.getElementsByName(name)：返回指定的`name`属性元素的一个即时NodeList对象
- document.getElementsByTagName(name)：返回HTML中该TagName的动态集合，数组，若无则为空HTMLCollection
- document.getElementsByClassName(names)：返回所有class含names的元素
- document.querySelector(selectors)：返回第一个匹配的元素，如果没有匹配的元素，则返回null
  - selectors为css选择器 .class #id 
- document.querySelectorAll(selectors)：可以同时传入多个css选择器，返回结果为非即时的NodeList
- element.matchersSelector()：调用元素与该选择符匹配，则返回true，否则返回false
- element.matches(selectorString)：调用元素如果包含selectorString这个css选择器则返回true

#### 节点关系型API

- 父关系型：
  - parentNode：元素的父节点
  - parentElemet：同parentNode，但必须是element节点否则返回null
  - node.contains( otherNode )：如果otherNod是 node的后代节点或是node节点本身.则返回true , 否则返回 false
- 子关系型API
  - childNodes：返回一个即时的NodeList，表示元素的子节点列表
  - children：一个即时的HTMLCollection，子节点都是Element节点
  - firstChild：返回节点中第一个子节点，若无则返回null
  - lastChild：返回当前节点的最后一个子节点，若无则返回null
  - hasChildNodes：返回一个布尔值，检测是否包含子节点
- 兄弟关系型API
  - previousSibling：返回当前节点的前一个兄弟节点，没有则返回null
  - previousElementSibling：同上，但是为element节点
  - nextSibling：返回当前节点的后一个兄弟节点，没有则返回null
  - nextElementSibling：同上，但是为element节点

#### 元素属性型API

- element.setAttribute(name, value)：name是属性名，value是属性值
- element.getAttribute(attributeName)：返回element节点上该name的属性值
- element.removeAttribute(attrName)：从元素中删除该属性名的属性

#### 元素样式型API

- Window.getComputedStyle()
- getBoundingClientRect()：用来返回元素的大小以及相对于浏览器可视窗口的位置

![img](https://user-gold-cdn.xitu.io/2018/6/5/163ceb8ce3986025?imageslim)



### HTML5新增

#### 与类相关的扩充

- getElementByClassName(names)：接收一个参数，包含一个或多个类名的字符串，返回所有带指定类元素的NodeList，只有位于调用元素的子树中的元素才会返回
- classList属性：className的增删改查
  - add(value)：添加类名
  - contains(value)：查询是否包含类名
  - remove(value)：删除类名
  - toggle(value)：如果列表中已经该类名，则删除，不存在则添加

- getAttribute：获取元素行间属性
- setAttribute：设置元素行间属性
- removeAttribute：删除元素行间属性
- clientWidth/clientHeight：获取元素宽高（不计算边框）
- offsetWidth/offsetHeight：获取元素宽高，带边框
- offsetTop: 获取当前元素外边框相对于父级内边框的高度
- document.documentElement.ClientWidth：获取可视区宽度
- document.documentElement.ClientHeight：获取可视区高度
- 使元素在页面中左右居中：拿到document的ClientWidth和元素的offsetWidth做差再/2，赋值给元素的left 