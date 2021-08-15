## 1.前言

所有的hooks都必须要用use*开头

hooks组件更加简洁、解码更快

**eslint针对hooks的插件：eslint-plugin-react-hooks**

## 2.useState

### 相关使用

useState的返回值是一个只有2个成员的数组

### 理论问题

**useState为什么可以保证是在当前组件**：js是单线程的，useState一定是在当前的上下文中

注意全局唯一性在hooks中的应用

### 注意事项

**useState是按照第一次运行时候的次序，对应的返回我们所声名的变量**

React要求每次的渲染useState都是按照稳定的数量和次序被调用的:因此必须在顶层被调用

useState中的函数只会执行一次，可以把声名默认值的操作放在里面

```jsx
const [value,setValue]=useState(()=>{
return props.defaultvalue
})
```

## 3.useEffect

### 副作用：

组件的渲染流程之外

绑定事件

发起异步请求

访问DOM元素

### 副作用的时机

Mount之后:componentDidMount

Update之后:componentDidUpdate

Unmount之前:componentWillUnmount

### useEffcet的运作

会在每次组件渲染之后进行调用

第一次调用：componentDidMount

更新/之后的调用都是更新：componentDidUpdate

Clean Callback:会返回一个回调函数，

- 回调函数的作用：清除上一次渲染的副作用遗留下来的状态，前一次的渲染视图被清除之前进行调用

如果useEffct只在第一次调用，回调函数就只会在组件卸载之前调用

### useEffect的使用注意事项

关注点分离：不同的事件最好用不同的useEffct写

useEffct回调函数的写法：

```jsx
useEffct(()=>{
//可以直接写事件逻辑
return ()=>{
//组件销毁的时候需要做的事情，第二个参数为[]的时候为销毁
}
}，[]) //clearn callback 只在销毁的之后执行的话，需要设置为空数组
```

### useEffect的第二个参数

useEffect第一次一定会执行，但是第二次什么时候执行，就需要设置第二个参数

示例：

```jsx
useEffct(()=>{

},[count]) //只有当count变化的时候useEffct才会执行
```

绑定的事件的DOM元素改变之后，之间就会失效，即使id是相同的

```jsx
const app=()=>{
    useEffct(()=>{
document.querySelector("#size").addEventListener("click",onchange,false)
    },[]) //这种情况下，当count值改变之后，dom元素改变了，onchange事件就不会起作用了，因此需要利用回调函数
        useEffct(()=>{
document.querySelector("#size").addEventListener("click",onchange,false)
            return ()=>{
                document.querySelector("#size").removeEventListener("click",onchange,false)
            }
    })//每次都清理之后，就可以再下一次执行的时候进行重新绑定
    
    return (
        <div>
            {
                count%2
                    ?<h1 id="size">hh</h1>
                    :<h2 id="size">hh</h2>
            }
        </div>
    )
}
```

## 3.useContext

解决问题：

- 函数组件不能使用contextType的问题
- contextType只能使用一个context的问题

### 使用

```jsx
//contextTpye的用法
class APP extends Component{
    static contextType=exampleContext; //只能用一次
render(){
   const count=this.context
   return(
   <div>
           {count}
   </div>
   )
}
}

//useEffct的用法
import React,{useContext} from "react"
const App=()=>{
    const count=useContext(exampleContext)//多定义几个也没问题
    return(
    <div>
           {count}
   </div>
    )
}
```

## 4.useMemo

memo的作用为判断一个组件是否需要重新渲染

useMemo的作用是判断一段函数是否需要重新执行 

以上的操作都是判断依赖是否有发生改变

### 优点

不会影响逻辑，只是用于性能优化

### 使用

useEffect执行的是副作用，因此一定是在渲染之后运行

useMemo是需要有返回值，并且会参与到渲染中，因此其是在渲染期间执行的

```jsx
const double=useMemo(()=>{
    return count*2
},[count===3])

//useMemo可以依赖useMemo的值
const third=useMemo(()=>{
    return count*2
},[double])
```

## 5.useCallback

useMemo返回的是一个函数，就等价于useCallback参数改为该函数

```jsx
useMemo(()=>fn)
useCallback(fn)
```

如果使用父元素传来的事件，即使组件被memo包裹，也会进行重新渲染，因此可以使用useMemo进行依赖绑定

![image-20210216223820001](D:\typora\images\image-20210216223820001.png)

**为什么这里可以不依赖clickCount!**

## 6.useRef

### 作用

相当于直接给了一个途径，可以直接获取得到组件或者DOM节点

变量也可以使用useRef:用来同步不同的渲染周期之间需要共享的数据

```jsx
const APP=()=>{
    let it=useRef();//如果使用的是直接定义，不使用useRef的话，渲染之后it变量发生改变，clearInterval不起作用
   useEffct(()=>{
       it.current=setInterval(()=>{
           count++
       },1000)
   },[])
   useEffct(()=>{
       if(count>10){
           clearInterval(it.current)}
   })
    return (
        <div ref={it}>hh</div>
    )
}
```

获取子组件或者DOM节点的句柄

渲染周期之间共享数据的存储（useState也可以，但是其数据的改变会触发重渲染，但是ref不会）

`ref` 会在 `componentDidMount` 或 `componentDidUpdate` 生命周期钩子触发前更新

## 使用

参考官网详细操作

https://zh-hans.reactjs.org/docs/refs-and-the-dom.html