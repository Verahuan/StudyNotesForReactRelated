## 1.前言

学习`umi`过程中，文件均为`.tsx`扩展名，所以先进行这个知识点的详细学习

`JSX`因[React](https://reactjs.org/)框架而流行。 `TypeScript`支持内嵌，类型检查以及将`JSX`直接编译为JavaScript。

## 2.基本用法

想要使用`JSX`必须做两件事：

1. 给文件一个`.tsx`扩展名
2. 启用`jsx`选项

`TypeScript`具有三种`JSX`模式：preserve，react和react-native

[tsconfig.json](https://www.tslang.cn/docs/handbook/tsconfig-json.html)中的选项来指定模式


```
"jsx": "react",//该模式直接会进行转换
```

这些模式只在代码生成阶段起作用 - 类型检查并不受影响

 在`preserve`模式下生成代码中会保留`JSX`以供后续的转换操作使用（比如：[Babel](https://babeljs.io/)）。 另外，输出文件会带有`.jsx`扩展名。 `react`模式会生成`React.createElement`，在使用前不需要再进行转换操作了，输出文件的扩展名为`.js`。 `react-native`相当于`preserve`，它也保留了所有的`JSX`，但是输出文件的扩展名是`.js`

![image-20210201151937376](C:\Users\16112\AppData\Roaming\Typora\typora-user-images\image-20210201151937376.png)