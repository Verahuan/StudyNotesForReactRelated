## 1.前言

可以用来复用状态逻辑

## 2.使用

自定义hooks必须use开头

自定义hooks和函数组件没有很大的区别，其输入和输出会有一定的不相同

hooks可以返回jsx进行渲染（可以当作组件来使用？?还是不要了）

## 3.优秀博客内容

### 前言

Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。本文是一篇**以实战为主的文章，主要讲解实际项目中如何使用hooks以及一些最佳实践**，不会一步步再介绍一遍react hooks的由来和基本使用，因为写hooks的文章很多，而且官网对于react hooks的介绍也很详细，所以大家不熟悉的可以看一遍[官网](https://zh-hans.reactjs.org/docs/hooks-intro.html)。

### 目录

- react hooks核心API使用以及注意事项
- 实现一个小型redux
- 实现自定义的useState
- 实现自定义的useDebounce
- 实现自定义的useThrottle
- 实现自定义useTitle
- 实现自定义的useUpdate
- 实现自定义的useScroll
- 实现自定义的useMouse
- 实现自定义的createBreakpoint

### 正文

### 1. react hooks核心API使用注意事项

笔者在项目中常用的hooks主要有**useState, useEffect，useCallback，useMemo，useRef**。当然像**useReducer, useContext, createContext**这些钩子在H5游戏中也会使用，因为不需要维护错综复杂的状态，所以我们完全可以由上述三个api构建一个自己的小型redux（后面会介绍如何实现小型的redux）来处理全局状态，但是对于企业复杂项目来说，我们使用redux及其生态会更加高效一些。

我们在使用hooks和函数组件编写我们的组件时，第一个要考虑的就是渲染性能，我们知道如果在不做任何处理时，我们在函数组件中使用setState都会导致组件内部重新渲染，一个比较典型的场景：

![img](D:\typora\images\170878b3e71017be)

当我们在容器组件手动更新了任何state时，容器内部的各个子组件都会重新渲染，为了避免这种情况出现，我们一般都会使用memo将函数组件包裹，来达到class组件的pureComponent的效果：



```jsx
import React, { memo, useState, useEffect } from 'react'
const A = (props) => {
  console.log('A1')
  useEffect(() => {
    console.log('A2')
  })
  return <div>A</div>
}

const B = memo((props) => {
  console.log('B1')
  useEffect(() => {
    console.log('B2')
  })
  return <div>B</div>
})

const Home = (props) => {
  const [a, setA] = useState(0)
  useEffect(() => {
    console.log('start')
    setA(1)
  }, [])
  return <div><A n={a} /><B /></div>
}
```

当我们将B用memo包裹后，状态a的更新将不会导致B组件重新渲染。其实仅仅优化这一点还远远不够的，比如说我们**子组件用到了容器组件的某个引用类型的变量或者函数，那么当容器内部的state更新之后，这些变量和函数都会重新赋值，这样就会导致即使子组件使用了memo包裹也还是会重新渲染**，那么这个时候我们就需要使用**useMemo**和**useCallback**了。

//常见hooks中涉及到该问题

**useMemo**可以帮我们将变量缓存起来，**useCallback**可以缓存回调函数，它们的第二个参数和useEffect一样，是一个依赖项数组，通过配置依赖项数组来决定是否更新。

```jsx
import React, { memo, useState, useEffect, useMemo } from 'react'
const Home = (props) => {
  const [a, setA] = useState(0)
  const [b, setB] = useState(0)
  useEffect(() => {
    setA(1)
  }, [])

  const add = useCallback(() => {
    console.log('b', b)
  }, [b])

  const name = useMemo(() => {
    return b + 'xuxi'
  }, [b])
  return <div><A n={a} /><B add={add} name={name} /></div>
}
```

此时a更新后B组件不会再重新渲染。以上几个优化步骤主要是用来优化组件的渲染性能，我们平时还会涉及到获取组件dom和使用内部闭包变量的情景，这个时候我们就可以使用**useRef**。

**useRef**返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。返回的 ref 对象在组件的整个生命周期内保持不变。

```jsx
function AutoFocusIpt() {
  const inputEl = useRef(null);
  const useEffect(() => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  }, []);
  return (
    <>
      <input ref={inputEl} type="text" />
    </>
  );
}
```

除了以上应用场景外，我们还可以利用它来实现class组件的setState的功能，具体实现后面会有介绍。

### 2. 实现一个小型redux

实现redux我们会利用之前说的**useReducer, useContext, createContext**这三个api，至于如何实现redux，其实网上也有很多实现方式，这里笔者写一个demo供大家参考：

```jsx
// actionType.js
const actionType = {
  INSREMENT: 'INSREMENT',
  DECREMENT: 'DECREMENT',
  RESET: 'RESET'
}
export default actionType

// actions.js
import actionType from './actionType'
const add = (num) => ({
    type: actionType.INSREMENT,
    payload: num
})

const dec = (num) => ({
    type: actionType.DECREMENT,
    payload: num
})

const getList = (data) => ({
    type: actionType.GETLIST,
    payload: data
})
export {
    add,
    dec,
    getList
}

// reducer.js
function init(initialCount) {
  return {
    count: initialCount,
    total: 10,
    user: {},
    article: []
  }
}

function reducer(state, action) {
  switch (action.type) {
    case actionType.INSREMENT:
      return {count: state.count + action.payload};
    case actionType.DECREMENT:
      return {count: state.count - action.payload};
    case actionType.RESET:
      return init(action.payload);
    default:
      throw new Error();
  }
}

export { init, reducer }

// redux.js
import React, { useReducer, useContext, createContext } from 'react'
import { init, reducer } from './reducer'

const Context = createContext()
const Provider = (props) => {
  const [state, dispatch] = useReducer(reducer, props.initialState || 0, init);
    return (
      <Context.Provider value={{state, dispatch}}>
        { props.children }
      </Context.Provider>
    )
}

export { Context, Provider }
复制代码
```

其实还有更优雅的方式实现，笔者之前也写了几套redux模版，欢迎一起讨论哈。接下来我们进入正文，来带大家实现几个常用的自定义hooks。

### 3. 实现自定义的useState，支持类似class组件setState方法

熟悉react的朋友都知道，我们使用class组件更新状态时，setState会支持两个参数，一个是更新后的state或者回调式更新的state，另一个参数是更新后的回调函数，如下面的用法：

```jsx
this.setState({num: 1}, () => {
    console.log('updated')
})
```

但是hooks函数的useState第二个参数回调支持类似class组件的setState的第一个参数的用法，并不支持第二个参数回调，但是很多业务场景中我们又希望hooks组件能支持更新后的回调这一方法，那该怎么办呢？其实问题也很简单，我们只要对hooks原理和api非常清楚的话，就可以通过自定义hooks来实现，这里我们借助上面提到的useRef和useEffect配合useState来实现这一功能。

> 注：react hooks的useState一定要放到函数组件的最顶层，不能写在if else等条件语句当中，来确保hooks的执行顺序一致，因为useState底层采用链表结构实现，有严格的顺序之分。

我们先来看看实现的代码：

```jsx
import { useEffect, useRef, useState } from 'react'

const useXState = (initState) => {
    const [state, setState] = useState(initState)
    let isUpdate = useRef()
    const setXState = (state, cb) => {
      setState(prev => {
        isUpdate.current = cb
        return typeof state === 'function' ? state(prev) : state
      })
    } //setState传入函数的话，传入的值是上一轮的状态，这里是state
    useEffect(() => {
      if(isUpdate.current) {
        isUpdate.current()
      }
    })
  
    return [state, setXState]
  }

export default useXState
```

笔者利用useRef的特性来作为标识区分是挂载还是更新，当执行setXstate时，会传入和setState一模一样的参数，并且将回调赋值给useRef的current属性，这样在更新完成时，我们手动调用current即可实现更新后的回调这一功能，是不是很巧妙呢？

### 4. 实现自定义的useDebounce

节流函数和防抖函数想必大家也不陌生，为了让我们在开发中更优雅的使用节流和防抖函数，我们往往需要让某个state也具有节流防抖的功能，或者某个函数的调用，为了避免频繁调用，我们往往也会采取节截流防抖这一思想，原生的节流防抖函数可能如一下代码所示：

```jsx
// 节流
function throttle(func, ms) {
    let previous = 0;
    return function() {
        let now = Date.now();
        let context = this;
        let args = arguments;
        if (now - previous > ms) {
            func.apply(context, args);
            previous = now;
        }
    }
}

// 防抖
function debounce(func, ms) {
    let timeout;
    return function () {
        let context = this;
        let args = arguments;
        if (timeout) clearTimeout(timeout);
        timeout = setTimeout(() => {
            func.apply(context, args)
        }, ms);
    }
}
复制代码
```

那么我们首先来实现一下防抖的hooks，代码如下：

```jsx
import { useEffect, useRef } from 'react'

const useDebounce = (fn, ms = 30, deps = []) => {
    let timeout = useRef()
    useEffect(() => {
        if (timeout.current) clearTimeout(timeout.current)
        timeout.current = setTimeout(() => {
            fn()
        }, ms)
    }, deps)

    const cancel = () => {
        clearTimeout(timeout.current)
        timeout = null
    }
    return [cancel]
  }

export default useDebounce
```

由代码可以知道，useDebounce接受三个参数，分别为回调函数，时间间隔以及依赖项数组，它暴露了cancel API，主要是用来控制何时停止防抖函数用的。具体使用如下：

```jsx
// ...
import { useDebounce } from 'hooks'
const Home = (props) => {
  const [a, setA] = useState(0)
  const [b, setB] = useState(0)
  const [cancel] = useDebounce(() => {
    setB(a)
  }, 2000, [a])

  const changeIpt = (e) => {
    setA(e.target.value)
  }
  return <div>
    <input type="text" onChange={changeIpt} />
    { b } { a }
  </div>
}
```

以上代码就实现了state的debounce的功能，具体效果如下图所示：

![img](D:\typora\images\17089ddf538f2a56)



### 5. 实现自定义的useThrottle

同理，我们继续来实现节流的hooks函数。直接上代码：

```
import { useEffect, useRef, useState } from 'react'

const useThrottle = (fn, ms = 30, deps = []) => {
    let previous = useRef(0)
    let [time, setTime] = useState(ms)
    useEffect(() => {
        let now = Date.now();
        if (now - previous.current > time) {
            fn();
            previous.current = now;
        }
    }, deps)

    const cancel = () => {
        setTime(0)
    }
  
    return [cancel]
  }

export default useThrottle
复制代码
```

代码和自定义useDebounce类似，但需要注意一点就是为了实现cancel功能，我们使用了内部state来处理，通过控制时间间隔来取消节流效果，当然还有很多其他方法可以实现这个hooks API。具体效果如下：

![img](D:\typora\images\17089f7c45af40bc)



### 6. 实现自定义useTitle

自定义的useTitle hooks其实使用场景也很多，因为我们目前大部分项目都是采用SPA或者混合SPA的方式开发，对于不同的路由我们同样希望想多页应用一样能切换到对应的标题，这样可以让用户更好的知道页面的主题和内容。这个hooks的实现也很简单，我们直接上代码：

```
import { useEffect } from 'react'

const useTitle = (title) => {
    useEffect(() => {
      document.title = title
    }, [])
  
    return
  }

export default useTitle
复制代码
```

以上代码可以看出我们只需要在useEffect中设置document的title属性就好了，我们不需要return任何值。其实还有更优雅和复杂的实现方法，这里就不一一举例了。具体使用如下：

```
const Home = () => {
    // ...
    useTitle('趣谈前端')
    
    return <div>home</div>
}
复制代码
```

### 7. 实现自定义的useUpdate

我们都知道如果想让组件重新渲染，我们不得不更新state，但是有时候业务需要的state是没必要更新的，我们不能仅仅为了让组件会重新渲染而强制让一个state做无意义的更新，所以这个时候我们就可以自定义一个更新的hooks来优雅的实现组件的强制更新，实现代码如下：

```
import { useState } from 'react'

const useUpdate = () => {
    const [, setFlag] = useState()
    const update = () => {
        setFlag(Date.now())
    }
  
    return update
  }

export default useUpdate
```

以上代码可以发现，我们useUpdate钩子返回了一个函数，该函数就是用来强制更新用的。使用方法如下：

```jsx
const Home = (props) => {
  // ...
  const update = useUpdate()
  return <div>
    {Date.now()}
    <div><button onClick={update}>update</button></div>
  </div>
}
```

效果如下：

![img](D:\typora\images\1708a3b3ec60fc4e)



### 8. 实现自定义的useScroll

自定义的useScroll也是高频出现的问题之一，我们往往会监听一个元素滚动位置的变化来决定展现那些内容，这个应用场景在H5游戏开发中应用十分广泛，接下来我们来看看实现代码：

```jsx
import { useState, useEffect } from 'react'

const useScroll = (scrollRef) => {
  const [pos, setPos] = useState([0,0])

  useEffect(() => {
    function handleScroll(e){
      setPos([scrollRef.current.scrollLeft, scrollRef.current.scrollTop])
    }
    scrollRef.current.addEventListener('scroll', handleScroll, false)
    return () => {
      scrollRef.current.removeEventListener('scroll', handleScroll, false)
    }
  }, [])
  
  return pos
}

export default useScroll
复制代码
```

由以上代码可知，我们在钩子函数里需要传入一个元素的引用，这个我们可以在函数组件中采用ref和useRef来获取到，钩子返回了滚动的x，y值，即滚动的左位移和顶部位移，具体使用如下：

```jsx
import React, { useRef } from 'react'

 import { useScroll } from 'hooks'
const Home = (props) => {
  const scrollRef = useRef(null)
  const [x, y] = useScroll(scrollRef)

  return <div>
      <div ref={scrollRef}>
        <div className="innerBox"></div>
      </div>
      <div>{ x }, { y }</div>
    </div>
}
复制代码
```

通过使用useScroll，钩子将会帮我们自动监听容器滚动条的变化从而实时获取滚动的位置。具体效果如下：

![img](D:\typora\images\1708a5fd80c9958a)



### 9. 实现自定义的useMouse和实现自定义的createBreakpoint

自定义的useMouse和createBreakpoint的实现方法和useScroll类似，都是监听窗口或者dom的事件来自动更新我们需要的值，这里我就不一一实现了，如果不懂的可以和我交流。通过这些自定义钩子能大大提高我们代码的开发效率，并将重复代码进行有效复用，所以大家在工作中可以多尝试。

当我们写了很多自定钩子时，一个好的开发经验就是统一管理和分发这些钩子，笔者建议可以在项目中单独建一个hooks的目录专门存放这些可复用的钩子，方便管理和维护。如下：

![img](D:\typora\images\1708a64d476bb121)