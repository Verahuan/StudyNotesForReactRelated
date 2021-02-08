如果dva中Effect.call同步数据，但是函数确实异步函数的话，会报错

![image-20210207112442276](D:\typora\images\image-20210207112442276.png)

![image-20210207112456311](D:\typora\images\image-20210207112456311.png)

可以获取到值，但是没办法正常渲染，解决方法

```javascript
effects:{
    *getRemote(action,effects){
      const data=yield effects.call(getRemoteList)
      console.log(data)
      yield effects.put({
        type:"getList",
        payload:{data} //解决：给data包裹一下
      })
    }
  },
```

具体是为什么其实还不清楚，需要继续了解

getRemoteList会返回一个promise对象