## devDependencies和dependencies的区别

dependencies：生产环境

devDependencies：开发环境

## 命令行相关

`npm install module-name -save` 

自动把模块和版本号添加到dependencies部分
`npm install module-name -save-dev` 

自动把模块和版本号添加到devdependencies部分

## 举例

```
假设有以下两个模块：
模块A
- devDependencies
  模块B
- dependencies
  模块C

模块D
- devDependencies
  模块E
- dependencies
  模块A

npm install D的时候， 下载的模块为：
- D
- A
- C

当我们下载了模块D的源码，并且在根目录下npm install， 下载的模块为：
- A
- C
- E
```

总结：

（生产相关依赖会一起下载）

在发布npm包的时候，本身dependencies下的模块会作为依赖，一起被下载；devDependencies下面的模块就不会自动下载了；但对于项目而言，npm install 会自动下载devDependencies和dependencies下面的模块。

