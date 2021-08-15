## 1.前言

部分使用不到的内容，可以先不使用

`webpack-Code Splitting`

`import`

`webpack`默认会对import动态导入的模块进行模块划分

## 2.介绍

执行组件的导入行为封装成为React组件

`React.lazy` 函数能让你像渲染常规组件一样处理动态引入（的组件）。

异步导入组件

**使用之前：**

```jsx
import OtherComponent from './OtherComponent';
```

**使用之后：**

`lazy`也可以使用解构导入的方式，如下`Suspense`的导入方法

```jsx
const OtherComponent = React.lazy(() => import(/*内部加的注释会和打包名一致*/'./OtherComponent'));
```

此代码将会在组件首次渲染时，自动导入包含 `OtherComponent` 组件的包。

`React.lazy` 接受一个函数，这个函数需要动态调用 `import()`。它必须返回一个 `Promise`，该 Promise 需要 resolve 一个 `default` export 的 React 组件。

然后应在 `Suspense` 组件中渲染 lazy 组件，如此使得我们可以使用在等待加载 lazy 组件时做优雅降级（如 loading 指示器等）。

```jsx
import React, { c } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );//Suspense对需要延迟加载的组件进行包裹
}
```

`fallback` 属性接受任何在组件加载过程中你想展示的 React 元素。你可以将 `Suspense` 组件置于懒加载组件之上的任何位置。你甚至可以用一个 `Suspense` 组件包裹多个懒加载组件。

```jsx
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

## 3.实现效果

可以实现将异步导入的组件加载进行打包，加载过程种显示suspense中的内容，同时设置有加载错误进行错误处理的功能

### 异常捕获边界（Error boundaries）

！参考代码分割和错误边界处理，官网

如果模块加载失败（如网络问题），它会触发一个错误。你可以通过[异常捕获边界（Error boundaries）](https://zh-hans.reactjs.org/docs/error-boundaries.html)技术来处理这些情况，以显示良好的用户体验并管理恢复事宜。

#### 类组件处理方法：

https://zh-hans.reactjs.org/docs/error-boundaries.html

#### 函数组件处理方法

待补

## 4.命名导出

`React.lazy` 目前只支持默认导出（default exports）。如果你想被引入的模块使用命名导出（named exports），你可以创建一个中间模块，来重新导出为默认模块。这能保证 tree shaking 不会出错，并且不必引入不需要的组件。

```jsx
// ManyComponents.js
export const MyComponent = /* ... */;
export const MyUnusedComponent = /* ... */;
// MyComponent.js
export { MyComponent as default } from "./ManyComponents.js";
// MyApp.js
import React, { lazy } from 'react';
const MyComponent = lazy(() => import("./MyComponent.js"));
```

