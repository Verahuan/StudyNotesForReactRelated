## 1.前言

文档地址:

这部分的主要内容：Redux Overview and Concepts

> - What Redux is and why you might want to use it
> - Key Redux terms and concepts
> - How data flows through a Redux app

因为是看英文文档和视频，所以笔记大部分是来源于英文文档，故为英文，部分地方会用中文注明，方便阅读。

官网的学习路径感觉还可以，跟着文档走，然后看完之后看一下作者的教学视频。

顺带提一下，官网涉及的这几个前期储备知识还是很有必要好好复习：

> - Familiarity with [HTML & CSS](https://internetingishard.com/).
> - Familiarity with [ES6 syntax and features](https://www.taniarascia.com/es6-syntax-and-feature-overview/)
> - Knowledge of React terminology: [JSX](https://reactjs.org/docs/introducing-jsx.html), [State](https://reactjs.org/docs/state-and-lifecycle.html), [Function Components, Props](https://reactjs.org/docs/components-and-props.html), and [Hooks](https://reactjs.org/docs/hooks-intro.html)
> - Knowledge of [asynchronous JavaScript](https://javascript.info/promise-basics) and [making AJAX requests](https://javascript.info/fetch)

## 2. 什么时候需要使用`Redux`

​	其实作为状态管理工具，既然作为工具，就需要考虑在合适的时机去使用它，针对一个实际的项目的时候，根据项目大小，数据量的大小以及数据的改变程度等都需要考虑到，官网给出的几个建议如下:

> - You have large amounts of application state that are **needed in many places** in the app
> - The app state is **updated frequently** over time
> - The logic to update that state may be **complex**
> - The app has a **medium or large-sized codebase,** and might be worked on by many people

以上几点其实说明，在有必要使用Redux的时候就去使用，不需要的时候也没有必要，同时还有参考阅读文章（之后项目有需求的时候进行学习，目前先以熟悉了解Redux为主，之后阅读）

> - **[When (and when not) to reach for Redux](https://changelog.com/posts/when-and-when-not-to-reach-for-redux)**
> - **[The Tao of Redux, Part 1 - Implementation and Intent](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/)**
> - **[Redux FAQ: When should I use Redux?](https://redux.js.org/faq/general#when-should-i-use-redux)**
> - **[You Might Not Need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)**

## 3.`Redux`的使用：`Redux` Libraries and Tools

`Redux`是一个小的独立`JS`库。但是，它通常与其他几个软件包一起使用：

#### `React-Redux`

 [**React-Redux**](https://react-redux.js.org/) is our official package that lets your React components interact with a Redux store by **reading pieces of state and dispatching actions to update the store**

就是作为一个`UI framework` ，React要使用`Redux`就需要使用该包

#### `Redux Toolkit`

[**Redux Toolkit**](https://redux-toolkit.js.org/) is our recommended approach for writing Redux logic. It contains packages and functions that we think are essential for building a Redux app. Redux Toolkit builds in our suggested best practices, simplifies most Redux tasks, prevents common mistakes, and makes it easier to write Redux applications.

工具包还是需要的，题词，减少语法错误，提高开发效率

#### `Redux DevTools Extension`

The [**Redux DevTools Extension**](https://github.com/zalmoxisus/redux-devtools-extension) shows a history of the changes to the state in your Redux store over time. This allows you to debug your applications effectively, including using powerful techniques like "time-travel debugging".

浏览器拓展程序，可以跟踪变化，在Vue里面也是有同样的功能的。

## 4. `Redux` Terms and Concepts

术语的理解可以对设计的思路有一定的了解。

### State Management(状态管理)

#### 了解单向数据流（**one-way data flow**）

同`Vue`

图示如下:

![One-way data flow](https://redux.js.org/assets/images/one-way-data-flow-04fe46332c1ccb3497ecb04b94e55b97.png)

描述方法如下:

三者介绍:

- The **state**, the source of truth that drives our app;

- The **view**, a declarative description of the UI based on the current state

- The **actions**, **the events** that occur in the app based on user input, and trigger updates in the state

  state和view其实都是实实在在有的概念(相当于是名词?),但是actions的话,其实就是动态的,触发更新的行为,其可以多种多样.

  > !看下面的术语部分,这里的actions理解有一点问题,需要结合下面的术语进行理解

单向数据流过程

- State describes the condition of the app at a specific point in time
- The UI is rendered based on that state
- When something happens (such as a user clicking a button), the state is updated based on what occurred(这里就是actions)
- The UI re-renders based on the new state

#### `Redux`的作用

However, the simplicity can break down when we have **multiple components that need to share and use the same state**, especially if those components are located in different parts of the application. Sometimes this can be solved by ["lifting state up"](https://reactjs.org/docs/lifting-state-up.html) to parent components, but that doesn't always help.

One way to solve this is to extract the shared state from the components, and put it into a centralized location outside the component tree. With this, our component tree becomes a big "view", and any component can access the state or trigger actions, no matter where they are in the tree!

**总结**:当组件中需要共享状态的时候,或者多个组件之间需要使用同一个状态的时候,就会出现比较混乱的情况,这个时候较好的方法就是抽离出需要被共享的状态.

#### `Redux`需要达到的状态

当然其首先就是需要可以进行状态管理,但是作为一个工具,首先就不需要对原来的项目产生过大的非必要影响,因此需要保证**其独立性**,并且使用起来需要**方便且可维护**

读源码的时候记得观察一下为什么具有这些点!

By defining and separating the concepts involved in state management and enforcing rules that maintain **independence** between views and states, we give our code **more structure and maintainability**.

### Immutability(不可变性)

**In order to update values immutably, your code must make copies of existing objects/arrays, and then modify the copies**.

不修改原来的对象或数组,而是创建副本之后修改副本,这里讲到一个知识点还挺有意思,就是在`JS`里面创建副本进行修改的方式.

文档给出了几种方法,但是其中就涉及到**深浅拷贝**的问题

方法如下:

```javascript
const obj = {
  a: {
    // To safely update obj.a.c, we have to copy each piece
    c: 3
  },
  b: 2
}

const obj2 = {
  // copy obj
  ...obj,
  // overwrite a
  a: {
    // copy obj.a
    ...obj.a,
    // overwrite c
    c: 42
  }
}

const arr = ['a', 'b']
// Create a new copy of arr, with "c" appended to the end
const arr2 = arr.concat('c')

// or, we can make a copy of the original array:
const arr3 = arr.slice()
// and mutate the copy:
arr3.push('c')
```

![image-20210129224829848](C:\Users\16112\AppData\Roaming\Typora\typora-user-images\image-20210129224829848.png)

说明解构符号就是浅拷贝,嘻嘻,其实其他的应该也是,但是我还没有都试过,留了问题在"待补问题了",后面统一解决~

### Terminology(Redux涉及的一些专业术语)

前面或多或少有涉及到,并且和vue里面这部分其实很类似,虽然操作不熟练,但是概念大概还是清楚的,所以还是要好好了解一下==

这部分就是概念问题,其实官网已经给的非常详细了~

#### Actions

An **action** is a **plain JavaScript object** that has a `type` field. **You can think of an action as an event that describes something that happened in the application**.

The `type` field should be a string that gives this action a descriptive name, like `"todos/todoAdded"`. We usually write that type string like `"domain/eventName"`, where the first part is the feature or category that this action belongs to, and the second part is the specific thing that happened.

An action object can have other fields with additional information about what happened. By convention, we put that information in a field called `payload`.

A typical action object might look like this:

```javascript
const addTodoAction = {
  type: 'todos/todoAdded',
  payload: 'Buy milk'
}
```

**总结**: Actions是一个对象,该对象用来描述当前的操作,该对象常见的键值"type"+"payload"

(看样子我上面的actions理解错了)

#### Action Creators

An **action creator** is a function

函数如下，可以返回一个Action 对象，方便多次使用

```
const addTodo = text => {
  return {
    type: 'todos/todoAdded',
    payload: text
  }
}
```

#### Reducers

A **reducer** is a function that receives the current `state` and an `action` object, decides how to update the state if necessary, and returns the new state: `(state, action) => newState`. **You can think of a reducer as an event listener which handles events based on the received action (event) type.**

**一个接收state和action的函数， handles events based on the received action (event) type**

Reducers must *always* follow some specific rules:

- They should only calculate the new state value based on the `state` and `action` arguments（可以给state对象新增加值吗？说是不可以，但是之后学的之后可以注意一下）
- They are not allowed to modify the existing `state`. Instead, they must make *immutable updates*, by copying the existing `state` and making changes to the copied values.（保证state的不变性，不对传入的state进行修改，而是创建副本）
- They must not do any asynchronous logic, calculate random values, or cause other "side effects"（这一步的话其实理解不是很深刻，知道会有影响，但是实际上的操作却不是很清楚，可以在之后的实践中多注意）

```javascript
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/increment') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  // otherwise return the existing state unchanged
  return state
}
```

#### Store

The current Redux application state lives in an object called the **store** .

The store is created by passing in a reducer, and has a method called `getState` that returns **the current state value**:

```javascript
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```

这个地方，返回当前的state,那么问题来了，什么是state，这个state和组件里面的state有什么区别？

> 应用中所有的 state 都以一个对象树的形式储存在一个单一的 *store* 中

啊！其实只能说有个理解，还是不知道具体的区别哦！加油！后面就会了！

#### Dispatch

The Redux store has a method called `dispatch`. **The only way to update the state is to call `store.dispatch()` and pass in an action object**. The store will run its **reducer function** and save the new state value inside, and we can call `getState()` to retrieve the updated value

```javascript
store.dispatch({ type: 'counter/increment' })

console.log(store.getState())
// {value: 1}
```

> reducer的函数是不是自己定义？
>
> reducer函数的定义是不是根据一种action定义一个
>
> state不是只有一个对象吗？是不是可以定义多个reduce，进行判断的时候怎么操作？

**You can think of dispatching actions as "triggering an event"**

Reducers act like event listeners, and when they hear an action **they are interested in**, they update the state in response

dispatch-告知有事件/触发事件

reducer-监听事件，看是不是可以更新的，是的就执行，不是就不

#### Selectors

**Selectors** are functions that know how to extract s**pecific pieces of information** from a store state value. As an application grows bigger, this can help avoid repeating logic as different parts of the app need to read the same data:

```javascript
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 2
```

为了取值方便==

## Redux Application Data Flow

基于单向数据流，有了redux之后一切都发生了变化，如下：

- Initial setup:

  - A Redux store is created using a **root reducer function**
  - The store calls the root reducer once, and saves the return value as its initial `state`
  - When the UI is first rendered, UI components access the current state of the Redux store, and use that data to decide what to render. They also **subscribe** to any future store updates so they can know if the state has changed.（这个地方订阅了）

- Updates:

  - Something happens in the app, such as a user clicking a button

  - The app code dispatches an action to the Redux store, like `dispatch({type: 'counter/increment'})`

  - The store runs **the reducer function** again with the previous `state` and the current `action`, and saves the return value as the new `state`（运行reducer-更改状态）

  - The store notifies all parts of the UI that are subscribed that the store has been updated（返回新的状态之后，通知订阅者更新界面）

  - **Each UI component** that needs data from the store checks to see if the parts of the state they need have changed.

    为什么是每个组件呢？不是有订阅吗，发布订阅不会直接通知到位？可能是我没有理解，到时候看看。

  - Each component that sees its data has changed forces a re-render with the new data, so it can update what's shown on the screen

![Redux data flow diagram](https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

## SUMMARY

- Redux is a library for managing global application state
  - Redux is typically used with the React-Redux library for integrating Redux and React together
  - Redux Toolkit is the recommended way to write Redux logic
- Redux uses a "one-way data flow" app structure
  - State describes the condition of the app at a point in time, and UI renders based on that state
  - When something happens in the app:
    - The UI dispatches an action
    - The store runs the reducers, and the state is updated based on what occurred
    - The store notifies the UI that the state has changed
  - The UI re-renders based on the new state
- Redux uses several types of code
  - *Actions* are plain objects with a `type` field, and describe "what happened" in the app
  - *Reducers* are functions that calculate a new state value based on previous state + an action
  - A Redux *store* runs the root reducer whenever an action is *dispatched*