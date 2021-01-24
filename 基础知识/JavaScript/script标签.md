### <script>

script存在几个可选属性

- async：可选，表示立即下载脚本，但是不妨碍页面中资源加载等
- defer：可选，表示脚本可以延迟到文档完全被解析和显示之后再执行

![请输入图片描述](http://segmentfault.com/img/bVcQV0)

区别：

- async（异步）：不会阻塞DOM树构建，加载完后立即执行
- defer（延迟）：不会阻塞DOM树构建，加载完后会等待DOM树构建完成后在执行
- 两者都不加则会阻塞DOM树构建