## 1.前言

练习demo中逐渐出现了生命周期的使用，这是否意味着我学的越来越多了

哈哈哈哈，就是这个道理，好的开始学习

**每个组件都包含 “生命周期方法”，你可以重写这些方法，以便于在运行过程中特定的阶段执行这些方法**。

其实和`Vue`差不多吧

至于重写生命周期方法的话，还没有涉及过，之后好好学习

## 挂载

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- [**`constructor()`**](https://zh-hans.reactjs.org/docs/react-component.html#constructor)

  在 React 组件挂载之前，会调用它的构造函数。在为 `React.Component` 子类实现构造函数时，应在其他语句之前前调用 `super(props)`。否则，`this.props` 在构造函数中可能会出现未定义的 bug。

  通常，在 React 中，构造函数仅用于以下两种情况：

  - 通过给 `this.state` 赋值对象来初始化[内部 state](https://zh-hans.reactjs.org/docs/state-and-lifecycle.html)。

  - 为[事件处理函数](https://zh-hans.reactjs.org/docs/handling-events.html)绑定实例

注意：
`setState()` 需要在constructor外进行调用

要避免在构造函数中引入任何副作用或订阅。如遇到此场景，请将对应的操作放置在 `componentDidMount` 中

:baby_symbol:那么对于Store的订阅是不是最好放在`componentDidMount`

**避免将 props 的值复制给 state！这是一个常见的错误：**

```
 super(props);
 // 不要这样做
 this.state = { color: props.color };
 //props更新的话，state的内容不会自动更新（有state依赖props的操作，请参阅关于避免派生状态的博文，以了解出现 state 依赖 props 的情况该如何处理）
 //如果需要使用props中的内容，可以直接取
}
```


- [`static getDerivedStateFromProps()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)

  `getDerivedStateFromProps` 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 `null` 则不更新任何内容。

- [**`render()`**](https://zh-hans.reactjs.org/docs/react-component.html#render)

  `render()` 方法是 class 组件中唯一必须实现的方法

  当 `render` 被调用时，它会检查 `this.props` 和 `this.state` 的变化并返回以下类型之一：

  - **React 元素**。通常通过 `JSX` 创建。例如，`<div />` 会被 React 渲染为 DOM 节点，`<MyComponent />` 会被 React 渲染为自定义组件，无论是 `<div />` 还是 `<MyComponent />` 均为 React 元素。

  - **数组或 fragments**。 使得 render 方法可以返回多个元素。欲了解更多详细信息，请参阅 [fragments](https://zh-hans.reactjs.org/docs/fragments.html) 文档。

  - **Portals**。可以渲染子节点到不同的 DOM 子树中。欲了解更多详细信息，请参阅有关 [portals](https://zh-hans.reactjs.org/docs/portals.html) 的文档

  - **字符串或数值类型**。它们在 DOM 中会被渲染为文本节点

  - **布尔类型或 `null`**。什么都不渲染。（主要用于支持返回 `test && <Child />` 的模式，其中 test 为布尔类型。)

    如需与浏览器进行交互，请在 `componentDidMount()` 或其他生命周期方法中执行你的操作

    `render()` 函数应该为**纯函数**，这意味着在不修改组件 state 的情况下，每次调用时都返回相同的结果，并且它不会直接与浏览器交互。

    目前使用的最多的还是范围jsx，其他的一些用法还需要再深入了解

    > 注意
    >
    > 如果 `shouldComponentUpdate()` 返回 false，则不会调用 `render()`

- [**`componentDidMount()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidmount)

  `componentDidMount()` 会在组件挂载后（插入 DOM 树中）立即调用。依赖于 DOM 节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。

  这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 `componentWillUnmount()` 里取消订阅

## 更新

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

- [`static getDerivedStateFromProps()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)

- [`shouldComponentUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)

  根据 `shouldComponentUpdate()` 的返回值，判断 React 组件的输出是否受当前 state 或 props 更改的影响。默认行为是 state 每次发生变化组件都会重新渲染。大部分情况下，你应该遵循默认行为。

  想要手动决定是否要进行，可以参考**[`PureComponent`](https://zh-hans.reactjs.org/docs/react-api.html#reactpurecomponent) 组件**

- [**`render()`**](https://zh-hans.reactjs.org/docs/react-component.html#render)

- [`getSnapshotBeforeUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)

- [**`componentDidUpdate()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentdidupdate)

  `componentDidUpdate()` 会在更新后会被立即调用。首次渲染不会执行此方法。

  当组件更新后，可以在此处对 DOM 进行操作。如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求。（例如，当 props 未发生变化时，则不会执行网络请求）。

  :package: 这部分其实涉及到不少知识点，可以在使用过程中再进一步学习

## 卸载

当组件从 DOM 中移除时会调用如下方法：

- [**`componentWillUnmount()`**](https://zh-hans.reactjs.org/docs/react-component.html#componentwillunmount)

`componentWillUnmount()` 会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 `componentDidMount()` 中创建的订阅等。

`componentWillUnmount()` 中**不应调用 `setState()`**，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。