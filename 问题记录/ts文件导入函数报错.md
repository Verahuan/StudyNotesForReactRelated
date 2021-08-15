## TS的模块导入出现的问题

首次报错:

```
object(...) is not a function
```

按照

 https://stackoverflow.com/questions/51997481/typeerror-object-is-not-a-function-reactjs?newreg=6f0bc982cac847568e80511f83ea1df7

介绍的方法，去掉模块导入的{}之后出现以下问题

```
TypeError: _hooks_useThrottle__WEBPACK_IMPORTED_MODULE_2___default(...) is not a function
```

参考网上的导入方法和配置修改不起作用，之后更改导入方法，明确文件类型之后解决问题

```
import useDebounce from "@/hooks/useThrottle.ts"
```

