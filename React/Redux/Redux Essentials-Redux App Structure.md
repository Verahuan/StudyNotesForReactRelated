## 1.前言

这部分其实是实际操作，在了解了相关的概念之后，进一步学习一下实际的操作会更好，这部分暂时不跟官网

## 2.在概念上的进一步理解

`Redux=Reducer+flux`

一个组件改变的store的内容，其他的组件都会去看自己是否需要更新。

Reducer根据传来的action执行其对应的行为

## 3.代码编写

使用ant-design+react

流程：

1. 入门文件：`index.js`

2. 定义`TodoList`组件

3. 引入`antD`布局

4. 使用`redux`

   安装`redux`

5. 创建store（`createStore`）

   store文件夹
   
   `index.js`文件
   
   `reducer.js`文件
   
   reducer传给store(创建stored的时候需要传入reducer)
   
6. reducer完善修改数据的逻辑
   
7. 组件需要订阅该store

   ```
   store.subscribe(this.aFunction)
   //store改变之后，aFunction就会执行
   ```

   ```
   this.setState(store.getState())
   //完全替换（没必要完全替换吧）
   ```

字符串换为变量

为了避免出现开发中的细节问题，可以淡出创建`actionType.js`文件（会报错，设置了变量）

`actioncreator`对action统一进行管理

reducer要是纯函数：给定固定的输入就会有固定的输出