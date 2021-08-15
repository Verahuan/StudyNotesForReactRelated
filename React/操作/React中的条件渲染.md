React 中的条件渲染有以下几种方式：

- if 语句
- 三元操作符(ternary operator)
- 逻辑 **&&** 操作符
- switch.. case.. 语句
- 枚举(enums)
- 多层条件渲染(multi-level conditional reandering)
- 使用高阶组件

## 1.if 语句

在React中使用if语句条件渲染是最简单的。比如List组件如果没有任何items,可以提前return



```jsx
function List({ list }) {
  if (!list) {
    return null;
  }
  return (
    <div>
      {list.map(item => <ListItem item={item} />)}
    </div>
  );
}
```

一个组件如果`return null`,将不会被渲染出来。如果你想给用户一些反馈，当list为空时，可以采用下面方式：



```jsx
function List({ list }) {
  // list 为null的情况
  if (!list) {
    return null;
  }

  // list 为空的情况
  if (!list.length) {
    return <p>sorry, the list is empty</p>
  } else {
    return (
      <div>
        {list.map(item => <ListItem item={item} />)}
      </div>
    );
  }
}
```

**if...else** 是最基本的条件渲染方式。

## 2.三元操作符

可以使用三元运算符来代替上面的`if...else...` 条件渲染。

例如，你想从2种模式(edit, view)中切换，可以使用布尔值来决定那个组件被渲染：



```jsx
function Item({ item, mode }) {
  const isEditMode = mode === 'EDIT';

  return (
    <div>
      { isEditMode
          ? <ItemEdit item={item} />
          : <ItemView item={item}>
      }
    </div>
  );
}
```

三元运算符使条件渲染更加的简洁,使得代码可以采用**内联**(inline)的方式表达出来

## 3.逻辑 '&&' 操作符

**这个是当你想渲染一个组件或者什么也不渲染的情况**。

例如，有一个 **`LoadingIndicator`** 组件,返回加载文字或者什么也没有。当然可以使用**if...else**或者**三元运算符**,如下：



```jsx
function LoadingIndicator({ isLoading }) {
  if (isLoading) {
    return (
      <div>
        <p>Loading...</p>
      </div>
    );
  } else {
    return null;
  }
}

function LoadingIndicator({ isLoading }) {
  return (
    <div>
      { isLoading
          ? <p>Loading...</p>
          : null
      }
    </div>
  );
}
```

**使用 `&&` 可以使返回 null 的情况的条件渲染更加的简洁**：



```jsx
function LoadingIndicator({ isLoading }) {
  return (
    <div>
      { isLoading && <p>Loading...</p> }
    </div>
  );
}
```

## 4.switch...case语句

我们有可能遇到多种条件渲染的情况，例如依据不同的state渲染不同的Component.

例如，**Notification** 组件根据输入状态可能渲染Error,Warning,Info三种不同的组件。这个时候可以使用switch...case来进行多种条件渲染：



```php
function Notification({ text, state }) {
  switch (state) {
    case 'info':
      return <Info text={text} />;
    case 'warning':
      return <Warning text={text} />;
    case 'info':
      return <Info text={text} />;
    default:
      return null;
  }
}
```

注意**switch...case**语句永远要加上**default**情况，因为React组件要么返回元素，要么返回null

另外值得注意的是，如果组件依据 *state* 属性 渲染时，最后添加上 **React.PropTypes**,即上面的组件后面添加：



```csharp
Notification.propTypes = {
  text: React.PropTypes.string,
  state: React.PropTypes.oneOf(['info', 'warning', 'error'])
}
```

**另一种将条件渲染结果内联在switch...case中的方法是，使用立即调用函数**



```php
function Notification({ text, state }) {
  return (
    <div>
      {(() => {
          switch (state) {
            case 'info':
              return <Info text={text} />;
            case 'warning':
              return <Warning text={text} />;
            case 'info':
              return <Info text={text} />;
            default:
              return null;
          }
      })()}
    </div>  
  );
}
```

`switch...case`帮助我们在多种条件中渲染中使用，但是多种条件渲染最好的方式是**枚举**

## 5.枚举(Enums)

在javascript中，对象的键值对可以用作枚举：



```cpp
const ENUM = {
  a: '1',
  b: '2',
  c: '3'
};
```

枚举是多种条件渲染中很好的一种方式，上面的 **Notification** 组件可以写为：



```xml
function Notification({ text, state }) {
  return (
    <div>
      {{
        info: <Info text={text} />,
        warning: <Warning text={text} />,
        error: <Error text={text} />
      }[state]}
    </div>
  )
}
```

上面的 *state* 属性用于取回对象中的值，这比switch...case可读性更强。

因为对象的值依赖 `text` 属性，所以我们必须使用内联对象。如果我们不需要text属性，我们可以使用外部静态枚举：



```jsx
const NOTIFICATION_STATES = {
  info: <Infor />,
  warning: <Warning />,
  error: <Error />,
};

function Notification({ state }) {
  return (
    <div>
      {NOTIFICATION_STATES[state]}
    </div>
  )
}
```

如果我们需要text属性，我们可以使用函数取回对象的值



```xml
const getSpecificNotification = (text) => ({
  info: <Info text={text} />,
  warning: <Warning text={text} />,
  error: <Error text={text} />,
});

function Notification({ state, text }) {
  return (
    <div>
      {getSpecificNotification(text)[state]}
    </div>  
  )
}
```

对象用作枚举，使用场景很灵活，下面例子：



```jsx
function FooBarOrFooOrBar({ isFoo, isBar }) {
  const key = `${isFoo}-${isBar}`;
  return (
    <div>
      {{
        ['true-true']: <FooBar />,
        ['true-false']: <Foo />,
        ['false-true']: <Bar />,
        ['false-false']: null,
      }[key]}
    </div>
  );
}
FooBarOrFooOrBar.propTypes = {
  isFoo: React.PropTypes.boolean.isRequired,
  isBar: React.PropTypes.boolean.isRequired
}
```

## 6.多层条件渲染

我们有时候可能碰到嵌套条件选择渲染的情况。

例如，List组件可能显示一个list,或者一个empty text提示，或者什么多没有：



```php
function List({ list }) {
  const isNull = !list;
  const isEmpty = !isNull && !list.length;

  return (
    <div>
      { isNull
          ? null
          : ( isEmpty
              ? <p>Sorry, the list is empty</p>
              : <div>{list.map(item => <ListItem item={item} />)}</div>
          )
      }
    </div>
  );
}

// 实例
<List list={null} /> // <div></div>
<List list={[]} /> // <div><p>Sorry, the list is empty</p></div>
<List list={['a', 'b', 'c']} /> // <div><div>a</div><div>b</div><div>c</div></div>
```

但是最好保持嵌套的层数最小化，这样代码可读性更强。**可以将组件划分成更小的组件的方式**



```php
function List({ list }) {
  const isList = list && list.length;

  return (
    <div>
      { isList
          ? <div>{list.map(item => <ListItem item={item} />)}</div>
          : <NoList isNull={!list} isEmpty={list && !list.empty} />
      }
    </div>
  );
}

function NoList({ isNull, isEmpty }) {
  return (!isNull && isEmpty) && <p>Sorry, the list is empty</p>
}
```

## 7.高阶组件用作条件渲染

**HOCs** 在React中很适合条件渲染。HOCs的一种使用方式就是改变组件的外观。

例如使用高阶组件来展示一个加载显示器或者一个想要的组件：



```php
function withLoadingIndicator(Component) {
  return function EnhancedComponent({ isLoading, ...props }) {
    if (!isLoading) {
      return <Component { ...props } />;
    }

    return <div><p>Loading...</p></div>;
  }
}

// 使用
const ListWithLoadingIndicator = withLoadingIndicator(List);

<ListWithLoadingIndicator
  isLoading={props.isLoading}
  list={props.list}
/>
```

这个示例中，List组件能关注渲染list上，而不必担心loading状态，另外，HOCs可以屏蔽list为null或empty的情况。

# 总结

使用上面的哪一种条件渲染可以根据下面的一些情况而定：

- if-else
  - 最简单的条件渲染
  - 适用于新手
  - 使用if,从渲染方法中返回null提前退出函数
- 三元操作符
  - 比if-else更好，优先使用
  - 比if-else更加简洁
- 逻辑 '&&' 操作符
  - 不返回元素就返回null时使用
- switch...case
  - 比较冗长
  - 可以通过立即调用函数内联使用
  - 使用枚举的方式代替这种方式更好
- 枚举(enums)
  - 使用于不同的状态使用
  - 超过一种条件选择时使用
- 多层次条件选择渲染
  - 避免使用这种方式对可读性的影响
  - 将组件划分为更小的组件，小组件自身带有条件选择
  - 偏向于使用高阶组件(HOCs)
- HOCs
  - 使用高阶组件而屏蔽条件渲染
  - 组件能关注主要的逻辑目的