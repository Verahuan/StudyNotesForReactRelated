## 1.定义

context提供了一种方式，能够让数组在组件树种传递，而不是一级一级传递

缺点：组件的复用性降低

## 2.使用

层级嵌套使用

父组件

```jsx
const newFeatures = () => {
  const [contextValue, SetContextValue] = useState(20)
  const [onlineValue, SetOnlineValue] = useState(false)
  return (
    <>
      <batteryContext.Provider value={contextValue}>
        <onlineContext.Provider value={onlineValue}>
          <FirstChild/>
        </onlineContext.Provider>
      </batteryContext.Provider>
      <div>zhh</div>
      <button
        type='button' onClick={()=>{
          SetContextValue(contextValue+1)
          SetOnlineValue(!onlineValue)
        }}>
        add Value
      </button>
    </>
  )
}
```

子组件

```jsx
const SecondChild=()=>{
  return(
    <batteryContext.Consumer>
      {
        value=>(
          <>
            <onlineContext.Consumer>
              {
                value=>(<div>this is online child <h1>online:{String(value)}</h1></div>)
              }
            </onlineContext.Consumer>
            <div>this is the second child <h1>value:{value}</h1></div>
          </>
        )
      }
    </batteryContext.Consumer>
  )
}
```

## 3.待解决问题

函数组件种使用`contextType`

