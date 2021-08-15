## 1.定义

context提供了一种方式，能够让数组在组件树中传递，而不是一级一级传递

Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法

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

```jsx
// Context 可以让我们无须明确地传遍每一个组件，就能将值深入传递进组件树。
// 为当前的 theme 创建一个 context（“light”为默认值）。
const ThemeContext = React.createContext('light');
class App extends React.Component {
  render() {
    // 使用一个 Provider 来将当前的 theme 传递给以下的组件树。
    // 无论多深，任何组件都能读取这个值。
    // 在这个例子中，我们将 “dark” 作为当前的值传递下去。
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// 中间的组件再也不必指明往下传递 theme 了。
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // 指定 contextType 读取当前的 theme context。
  // React 会往上找到最近的 theme Provider，然后使用它的值。
  // 在这个例子中，当前的 theme 值为 “dark”。
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

