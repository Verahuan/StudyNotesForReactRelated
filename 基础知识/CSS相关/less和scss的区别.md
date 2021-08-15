# [Sass/Scss 和 Less的区别](https://www.cnblogs.com/wangpenghui522/p/5467560.html)

### 一. Sass/Scss、Less是什么?

Sass (Syntactically Awesome Stylesheets)是一种动态样式语言，Sass语法属于缩排语法，比css比多出好些功能(如变量、嵌套、运算,混入(Mixin)、继承、颜色处理，函数等)，更容易阅读。

Sass的缩排语法，对于写惯css前端的web开发者来说很不直观，也不能将css代码加入到Sass里面，因此Sass语法进行了改良，Sass 3就变成了Scss(Sassy CSS)。SCSS(Sassy CSS)是CSS语法的扩展。这意味着每一个有效的CSS也是一个有效的SCSS语句，与原来的语法兼容，只是用{}取代了原来的缩进。

Less也是一种动态样式语言. 对CSS赋予了动态语言的特性，如变量，继承，运算， 函数.  Less 既可以在客户端上运行 (支持IE 6+, Webkit, Firefox)，也可在服务端运行 (借助 Node.js)。

### 二. Sass/Scss与Less区别

#### 1.编译环境不一样

Sass是在服务端处理的，以前是Ruby，现在是Dart-Sass或Node-Sass，而Less是需要引入less.js来处理Less代码输出CSS到浏览器，也可以在开发服务器将Less语法编译成css文件，输出CSS文件到生产包目录，有npm less, Less.app、SimpleLess、CodeKit.app这样的工具，也有在线编译地址。

#### 2.变量符不一样，Less是@，而Scss是$。

```
Less-变量定义
@color: #00c; /* 蓝色 */
#footer {
  border: 1px solid @color; /* 蓝色边框 */
}
scss-变量定义
$color: #00c; /* 蓝色 */

#footer {
  border: 1px solid $color; /* 蓝色边框 */
}
```

#### 3.输出设置，Less没有输出设置，Sass提供4中输出选项：nested, compact, compressed 和 expanded。

输出样式的风格可以有四种选择，默认为nested

- nested：嵌套缩进的css代码
- expanded：展开的多行css代码
- compact：简洁格式的css代码
- compressed：压缩后的css代码

#### 4.Sass支持条件语句，可以使用if{}else{},for{}循环等等。而Less不支持。

##### 4.1 if-else if-else示例：

```
@mixin txt($weight) { 
  color: white; 
  @if $weight == bold { 
    font-weight: bold;
  } 
  @else if $weight == light { 
    font-weight: 100;
  } 
  @else { 
    font-weight: normal;
  } 
}

.txt1 { 
  @include txt(bold); 
}
```

编译结果：

```
.txt1 {
  color: white;
  font-weight: bold; 
}
```

##### 4.2 for示例：

```
@for $i from 1 to 10 {
  .border-#{$i} {
    border: #{$i}px solid blue;
  }
}
```

编译结果:

```
.border-1 {
  border: 1px solid blue; }

.border-2 {
  border: 2px solid blue; }

.border-3 {
  border: 3px solid blue; }

.border-4 {
  border: 4px solid blue; }

.border-5 {
  border: 5px solid blue; }

.border-6 {
  border: 6px solid blue; }

.border-7 {
  border: 7px solid blue; }

.border-8 {
  border: 8px solid blue; }

.border-9 {
  border: 9px solid blue; }
```

####  5. 引用外部CSS文件

scss@import引用的外部文件如果不想编译时多生成同名的.css文件，命名必须以_开头, 文件名如果以下划线_开头的话，Sass会认为该文件是一个引用文件，不会将其编译为同名css文件.

```
// 源代码：
@import "_test1.scss";
@import "_test2.scss";
@import "_test3.scss";
// 编译后：
h1 {
  font-size: 17px;
}
 
h2 {
  font-size: 17px;
}
 
h3 {
  font-size: 17px;
}
 
```

Less引用外部文件和css中的@import没什么差异。

#### 6.Sass和Less的工具库不同

Sass有工具库Compass, 简单说，Sass和Compass的关系有点像Javascript和jQuery的关系,Compass是Sass的工具库。在它的基础上，封装了一系列有用的模块和模板，补充强化了Sass的功能。

Less有UI组件库Bootstrap,Bootstrap是web前端开发中一个比较有名的前端UI组件库，Bootstrap的样式文件部分源码就是采用Less语法编写。

 

#### 7.安装体验不同

用npm 或者yarn 安装less非常容易，而安装sass, 在国内没有翻墙的话，要么费了九牛二虎之力才能安装成功，要么就一直报安装失败。安装体验磕磕绊绊，很差劲。

 

### 三. 总结

不管是Sass，还是Less，都可以视为一种基于CSS之上的高级语言，其目的是使得CSS开发更灵活和更强大，Sass的功能比Less强大,基本可以说是一种真正的编程语言了，Less则相对清晰明了,易于上手,对编译环境要求比较宽松。考虑到编译Sass要安装Ruby,而Ruby官网在国内访问不了,个人在实际开发中更倾向于选择Less。