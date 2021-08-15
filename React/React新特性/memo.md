## 1.前言

`pureComponet`和`memo`都可以优化组件的重渲染行为，不发生改变就不进行渲染

React需要数据与视图同步

一旦页面的数据变化，页面就会重新渲染

生命周期函数

`shouldComponetUpdate`生命周期函数可以判断

## `pureComponet`

`pureComponet`:子组件继承`pureComponet`

只有传入的数据的第一级发生变化才会进行重新渲染，如果是对象的话，不会发生改变

## `memo`

函数组件想要使用`pureComponet`的功能，就需要用memo包裹

## 相关问题

`pureComponent`的对比过程，是怎么进行对比的

文章中结合源码进行分析了

https://juejin.cn/post/6844903806170300423