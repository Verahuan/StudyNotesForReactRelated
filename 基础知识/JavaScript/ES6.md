### ES6常见新特性

let/Const、箭头函数、模板字符串、解构赋值、函数参数默认值/Rest参数、扩展运算符、Map Set、Proxy、Reflect、Symbols、Promise、Iterator+For of、Generators、Class

---

### let var const

主要区别：

- 块级作用域
- 不存在变量提升
  - 并非真的不会提升，只是在声明以前不可用，即暂时性死区TDZ
- 暂时性死区
  - 会导致typeof变的会报错，在TDZ里typeof报错ReferenceError
- 不可重复声明
- let、const声明的全局变量不会挂到window下面

const注意点

- const 声明之后必须马上赋值，否则会报错
- const 简单类型一旦声明就不能再更改，复杂类型(数组、对象等)指针指向的地址不能更改，内部数据可以更改
- 如果想完全冻结对象，使用Object.freeze(obj)，则属性也无更改

块级作用域与函数声明

- ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明
- ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用
  - 以上只对支持ES6的浏览器有用，如果不支持，还是将函数声明当做var提升
- 鉴于以上问题，块级作用域函数声明容易出bug，所以**在块级作用域声明函数，最好使用匿名函数的形式**：**let** a = **function** () {}，这样会使函数作用域比较清晰

---

### 解构赋值

数组解构赋值：左边加[ ]

- let [foo, [[bar], baz]] = [1, [[2], 3]]   // 123
- let [x, , y] = [1, 2, 3]   // 1 3
- let [head, ...tail] = [1, 2, 3, 4]  // 1 [2,3,4]
- 默认值  let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
- 右侧为不可遍历结构则均报错
- 解构不成功，变量值为undefined

对象解构赋值

- let { foo, bar } = { foo: 'aaa', bar: 'bbb' }   // foo  "aaa"  bar  "bbb"
- let { foo: baz } = { foo: 'aaa', bar: 'bbb' }  // baz  "aaa" foo // error: foo is not defined
  - `foo`是匹配的模式，`baz`才是变量，赋值的是变量，模式只是用来匹配的
- 嵌套结构的对象一样可以，按照对象的嵌套模式解构即可
- 对已经声明过的变量进行解构赋值，需要将整个赋值表达式用（）括起来
- 默认值  var {x: y = 3} = {x: 5}  // y 5  x是模式，y是变量被赋值   
- 解构不成功，变量值为undefined

字符串解构赋值

- 字符串被当做类数组对象解构 const [a, b, c, d, e] = 'hello'

数值和布尔值的解构赋值

- 解构赋值时，如果等号右边是数值和布尔值，则会先转为对象
  - let {toString: s} = 123  s === Number.prototype.toString // true
- 解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于`undefined`和`null`无法转为对象，所以对它们进行解构赋值，都会报错

函数参数的解构赋值

- [[1, 2], [3, 4]].map(([a, b]) => a + b);   // [ 3, 7 ]

- ```javascript
  // 参数默认值情况
  function move({x = 0, y = 0} = {}) {
    return [x, y];
  }
  
  move({x: 3, y: 8}); // [3, 8]
  move({x: 3}); // [3, 0]
  move({}); // [0, 0]
  move(); // [0, 0]
  
  // 变量默认值情况
  function move({x, y} = { x: 0, y: 0 }) {
    return [x, y];
  }
  
  move({x: 3, y: 8}); // [3, 8]
  move({x: 3}); // [3, undefined]
  move({}); // [undefined, undefined]
  move(); // [0, 0]
  ```

-  区别总结：参数默认值情况是函数参数有默认值，所以不给也有值；而对象默认值情况是对象有默认值，如果赋值空对象即改变了它的默认对象，参数变成undefined

用途

- **交换变量的值**：不需要第三者[x, y] = [y, x]
- **从函数返回多个值**：返回对象或者数组，然后接受返回值解构
- **函数参数的定义**：解构赋值可以方便地将一组参数与变量名对应起来
- **提取 JSON 数据**：解构赋值对提取 JSON 对象中的数据
- **函数参数的默认值**
- **遍历 Map 结构**：for (let [key, value] of map)  直接取键值
- **输入模块的指定方法**：从模块中导出部分方法，类似对象的解构

---

### 字符串、正则、数值扩展

字符串

- 模板字符串 `foo ${变量名、函数调用} bar`
- **includes()**：返回布尔值，表示是否找到了参数字符串
- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部
- **repeat()**：重复，'na'.repeat(3) // 'nanana'，参数是 0 到-1 之间的小数，则等同于 0
- **padStart()**：补全头部，'x'.padStart(4, 'ab') // 'abax'，默认用空格补
- **padEnd()**：补全尾部：'x'.padEnd(4, 'ab') // 'xaba'，默认用空格补
- **trimStart()**：消除字符串头部的空格，返回新字符串，不修改
- **trimEnd()**：消除尾部的空格，返回新字符串，不修改
- **matchAll()**：方法返回一个正则表达式在当前字符串的所有匹配

正则

- ES6允许RegExp构造函数第一个参数是正则对象，第二个参数指定修饰符
- “先行断言”：x只有在y前面才匹配，必须写成/x(?=y)/
- “先行否定断言”：x只有不在y前面才匹配，必须写成/x(?!y)/
- “后行断言”：x只有在y后面才匹配，必须写成/(?<=y)x/
- “后行否定断言”：x只有不在y后面才匹配，必须写成/(?<!y)x/
- 具名组匹配：
  - 非具名：const RE_DATE = /(\d{4})-(\d{2})-(\d{2})/;  以数组index取结果
  - 具名：const RE_DATE = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;  以属性名取值
- y修饰符：也是全局匹配，与g的区别在于确保匹配必须从剩余的第一个位置开始，这也就是“粘连”的涵义
- matchAll()：可以一次取出全局匹配结果数组，不需要通过循环操作

数值

- Number.isFinite()：检查一个数值是否为有限的（finite），即不是`Infinity`，非数值直接false
- Number.isNaN()：用来检查一个值是否为`NaN`，非数值直接false
  - 传统的全局方法`isFinite()`和`isNaN()`会对非数值先进行类型转换
- Number.parseInt、Number.parseFloat：跟全局方法parseInt一样，只是减少全局方法使语言逐步模块化
- Number.isInteger()：用来判断一个数值是否为整数
- Number.EPSILON：极小常量，表示 1 与大于 1 的最小浮点数之间的差
- Number.isSafeInteger()：安全整数，范围在`-2^53`到`2^53`之间（不含两个端点）
- Math.trunc：用于去除一个数的小数部分
- Math.sign：用来判断一个数到底是正数（返回+1）、负数（-1）、还是零（0）、其他值（NaN）
- Math.cbrt：用于计算一个数的立方根
- Math.hypot：方法返回所有参数的平方和的平方根
- `**`：指数运算，a ** b = a的b次方
- BigInt：第八种数据类型，可表示2^53以外

### 函数、数组、对象扩展

函数

- 参数默认值

  - 使用参数默认值时，函数不能有同名参数
  - 与解构赋值共同使用
  - 通常给参数默认值的都是尾参数，否则不能省略传参

- 函数length属性：返回指定了默认值的参数之前的形参个数

- 作用域：设置参数默认值后，参数会有个单独的作用域

  ```javascript
  var x = 1;
  function f(x, y = x) {
    console.log(y);
  }
  f(2) // 2
  
  let x = 1;
  function f(y = x) {
    let x = 2;
    console.log(y);
  }
  f() // 1
  ```

- rest参数：扩展运算符获取多余参数，形式为`...变量名`

- name属性：返回函数名，ES5匿名函数名为空，ES6为f

- 箭头函数，不适用定义对象方法、需要动态this的时候

  - 嵌套箭头函数：前一个函数的输出是后一个函数的输入

- 尾调用优化：在尾部调用即可不需要保存外层函数的调用栈

  - 尾递归：尾部递归，不存在栈溢出错误

    ```javascript
    // 尾递归优化版 斐波那契数列
    function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
      if( n <= 1 ) {return ac2};
      return Fibonacci2 (n - 1, ac2, ac1 + ac2);
    }
    ```

- ES6的尾调用优化只能在严格模式下开启，但是arguments、caller参数会失效
- 函数实例的`toString()`改写，以前只能返回代码，省略注释空格，现在会返回一样的原始代码

数组

- 扩展运算符
  - 复制数组：[...a2] = a1、a2 = [...a1]， = a1.concat() = a1.slice()
  - 合并数组：[...arr1, ...arr2, ...arr3] = arr1.concat(arr2, arr3)
  - 解构赋值：获取剩余参数，形成数组，只能放在最后
  - 字符串转数组：[...'hello'] = 'hello'.split("")
  - 任何Iterator接口的均可使用扩展运算符转成真正的数组，如Map、Set
- `Array.from`：用于将两类对象转为真正的数组
- `Array.of`：用于将一组值，转换为数组
- copyWithin()：在当前数组内部，将指定位置的成员复制到其他位置，具体见数组API
- find()和findIndex()：用于找出第一个符合条件的数组成员、返回第一个成员位置
- fill：使用给定值，填充一个数组
- entries()：返回数组键值对
- keys()：返回数组键名
- values()：返回数组里键值
- includes()：方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的`includes`方法类似
- flat()：数组扁平化，传参为几就扁平化几层，默认值1层，小于等于0则不拉
- flatMap()：对原数组的每个成员先map，再执行flat，只能展开一层
- 数组的空位：Array(3) // [, , ,]，空位并不是undefined，空位是没有任何值

对象

- 属性简写，ES6允许大括号里直接写入变量和函数作为对象的属性和方法

- 属性名表达式：属性名可以['a' + 'b'] 这种表示

- 方法的name属性：即函数的name

- 属性的可枚举性和遍历：

| 方法                         | 说明                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| For in                       | 遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）         |
| Object.keys                  | 返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名 |
| Object.getOwnPropertyNames   | 返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名 |
| Object.getOwnPropertySymbols | 返回一个数组，包含对象自身的所有 Symbol 属性的键名           |
| Reflect.ownKeys              | 返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举 |

- super关键字：this总是指向函数所在的当前对象，super指向当前对象的原型，即proto
- 对象扩展运算符：基本同数组，但是如果右边是null、undefined，则报错
- 链判断运算符：`message?.body?.user?.firstName || 'default'`，左侧对象为null或者undefined时，直接不再往下运算返回undefined
  - 短路机制：左侧为null或者undefined后，右侧不再继续
  - 括号的影响： (a?.b).c = (a == null ? undefined : a.b).c，对外部无影响
- Null判断符?？：headerText ?? 'Hello, world!'，只有左侧为null和undefined右边才会生效，false，0不会触发
- Object.is()：比===更严格，解决了+0 = - 0、NaN不等于自身的bug
- Object.assign()：用于合并对象，将多个对象的属性合并成一个对象，如果有重复则后者覆盖前者
  - 第一个参数为目标对象，后面均为源对象
  - 首参数非对象先转成对象，无法转则跳过，非首参的，不能转化对象直接报错（null、undefined）
  - 如果只有一个参数，则直接返回该参数，如果非对象，则转成对象
  - 数值、字符串和布尔值）不在首参数，也不会报错，只有字符串以数组形式拷入，其他的直接忽略
  - 该方法等于浅拷贝
- Object.getOwnPropertyDescriptors()：返回指定对象所有自身属性（非继承属性）的描述对象
- `__proto__`：用来读取或设置当前对象的原型对象（prototype）
- Object.setPrototypeOf：Object.setPrototypeOf(object, prototype)，作用与`__proto__`相同，用来设置一个对象的原型对象（prototype），返回参数对象本身
- Object.setPrototypeOf：Object.getPrototypeOf(obj)，用于读取一个对象的原型对象
- `Object.keys`：返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名
- `Object.values`：返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值，会过滤属性名为 Symbol 值的属性
- `Object.entries()`：返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组
- `Object.fromEntries()`方法是`Object.entries()`的逆操作，用于将一个键值对数组转为对象

---

### Symbol

ES5 的对象属性名都是字符串，容易造成属性名的冲突，`Symbol`，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，let s = Symbol()，直接typeof检测，不能new

`Symbol`函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分

Symbol值不能与其他类型的值进行运算，但是可以显示转成字符串或者布尔值

Symbol值可以做对象的属性名，但是只能用`[]`来表示，不能用点运算法

想获取对象Symbol键名：

- Object.getOwnPropertySymbols() ：可以获取所有 Symbol 属性名
- Reflect.ownKeys()：可以返回所有类型的键名，包括常规键名和 Symbol 键名

创建symbol

- 正常创建：`let s = Symbol()`
- 创建带描述的Symbol：`let s = Symbol('foo')`
- 创建相同值的Symbol：`let s1 = Symbol.for('foo')`，以foo为key创建Symbol，如果已经有同名的key，则取已经存在的值
  - 获取Symbol的key值，let s1 = Symbol.for("foo");    Symbol.keyFor(s1) // "foo"

---

### Set和Map

Set

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值

- 去重：数组 [...new Set(array)]     字符串 [...new Set('ababbc')].join('')
- Set内部判断重复是以===判断，所以会NaN = NaN

实例属性：

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数
- `Set.prototype.size`：返回`Set`实例的成员总数

方法：

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员
- `Set.prototype.clear()`：清除所有成员，没有返回值

遍历：由于Set无键名，只有键值，所以keys和values返回一致

- `Set.prototype.keys()`：返回键名的遍历器
- `Set.prototype.values()`：返回键值的遍历器
- `Set.prototype.entries()`：返回键值对的遍历器
- `Set.prototype.forEach()`：使用回调函数遍历每个成员
- 扩展运算符

WeakSet

WeakSet相比于Set，区别：

- 成员只能是对象，
- WeakSet中引用为弱引用，即垃圾回收机制不考虑其中引用

方法

- **WeakSet.prototype.add(value)**：向 WeakSet 实例添加一个新成员
- **WeakSet.prototype.delete(value)**：清除 WeakSet 实例的指定成员
- **WeakSet.prototype.has(value)**：返回一个布尔值，表示某个值是否在 WeakSet 实例之中

Map

JS中的对象本质是键值对的集合（Hash结构），但是传统上只能用字符串当键，Map不受此限制

键名相同时，后者会覆盖前者；由于键名可以为对象，需要注意以**对象为键名时内存地址不同并不是同一个值**，即为两个键

- Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。这就解决了同名属性碰撞（clash）的问题

实例属性：

- `size`：属性返回 Map 结构的成员总数
- `Map.prototype.set(key, value)`： `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构，重复设置同key值会更新
- `Map.prototype.get(key)`：get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined
- `Map.prototype.has(key)`：`has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中
- `Map.prototype.delete(key)`：delete`方法删除某个键，返回`true`。如果删除失败，返回`false
- `Map.prototype.clear()`：`clear`方法清除所有成员，没有返回值

遍历（同set）

- `Map.prototype.keys()`：返回键名的遍历器
- `Map.prototype.values()`：返回键值的遍历器
- `Map.prototype.entries()`：返回所有成员的遍历器
- `Map.prototype.forEach()`：遍历 Map 的所有成员

| 数据结构转换 | 转                               | 转Map                        |
| ------------ | -------------------------------- | ---------------------------- |
| 数组         | 扩展运算符                       | new Map( arr )               |
| 对象         | let [k,v] of strMap； obj[k] = v | new Map(Object.entries(obj)) |
| JSON         | JSON.stringify                   | JSON.parse                   |

WeakMap（深拷贝，考虑循环引用时使用）

结构如同Map，但区别

- 只接受对象作为键名（null除外）
- 弱引用，不计入垃圾回收机制
- 无遍历操作，也没size属性
- 无法清空，不支持clear方法

---

### Proxy

Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

```javascript
var obj = new Proxy({}, {
  get: function (target, propKey, receiver) {
    console.log(`getting ${propKey}!`);
    return Reflect.get(target, propKey, receiver);
  },
  set: function (target, propKey, value, receiver) {
    console.log(`setting ${propKey}!`);
    return Reflect.set(target, propKey, value, receiver);
  }
});
// var proxy = new Proxy(target, handler);
// target参数表示所要拦截的目标对象，handler参数也是一个对象，用来定制拦截行为
```

Proxy支持的拦截操作（具体介绍见 阮一峰ES6）

- **get(target, propKey, receiver)**：拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`
- **set(target, propKey, value, receiver)**：拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值
- **has(target, propKey)**：拦截`propKey in proxy`的操作，返回一个布尔值
- **deleteProperty(target, propKey)**：拦截`delete proxy[propKey]`的操作，返回一个布尔值
- **ownKeys(target)**：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性
- **getOwnPropertyDescriptor(target, propKey)**：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象
- **defineProperty(target, propKey, propDesc)**：拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值
- **preventExtensions(target)**：拦截`Object.preventExtensions(proxy)`，返回一个布尔值
- **getPrototypeOf(target)**：拦截`Object.getPrototypeOf(proxy)`，返回一个对象
- **isExtensible(target)**：拦截`Object.isExtensible(proxy)`，返回一个布尔值
- **setPrototypeOf(target, proto)**：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截
- **apply(target, object, args)**：拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`
- **construct(target, args)**：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`

Proxy.revocable()

- 返回一个可取消的Proxy实例，let {proxy, revoke} = Proxy.revocable(target, handler)
- 该对象的`proxy`属性是`Proxy`实例，`revoke`属性是一个函数，可以取消`Proxy`实例
- `Proxy.revocable()`的一个使用场景是，目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问

this问题

- 在 Proxy 代理的情况下，目标对象内部的`this`关键字会指向 Proxy 代理

```javascript
const target = {
  m: function () {
    console.log(this === proxy);
  }
};
const handler = {};

const proxy = new Proxy(target, handler);

target.m() // false
proxy.m()  // true
// 一旦proxy代理target.m，后者内部的this就是指向proxy，而不是target
```

---

### Reflect

也是 ES6 为了操作对象而提供的新 API，设计目的如下

- 将`Object`对象的一些明显属于语言**内部的方法**（比如`Object.defineProperty`），放到`Reflect`对象上
- **修改某些`Object`方法的返回结果**，让其变得更合理，比如Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false
-  让`Object`**操作都变成函数行为**。某些`Object`操作是命令式，比如`name in obj`和`delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为
- `Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法，即**Reflect会保存Proxy代理前的原方法**

静态方法：

- 一共13个静态方法，与Proxy一一对应

Proxy实现观察者模式（ES6入门上的例子）

```javascript
const person = observable({
  name: '张三',
  age: 20
});

function print() {
  console.log(`${person.name}, ${person.age}`)
}

observe(print);
person.name = '李四';
// person是观察目标，函数print是观察者
// 李四, 20

// proxy实现
const queuedObservers = new Set();  //观察者队列

const observe = fn => queuedObservers.add(fn);  //添加观察者函数
const observable = obj => new Proxy(obj, {set});  // 代理观察目标

function set(target, key, value, receiver) {  //对象 name属性 值 this绑定receiver
  const result = Reflect.set(target, key, value, receiver); //代理
  queuedObservers.forEach(observer => observer()); //触发观察者，逐个执行
  return result;
}
```

---

### Iterator

JS中四种"集合"数据结构：数组、对象、Map、Set

遍历器Iterator是一种接口，为各种不同的数据结构提供统一的访问机制，任何数据结构只要部署Iterator接口，就可以完成遍历操作

Iterator 的作用有三个：

- 为各种数据结构，提供一个统一的、简便的访问接口
- 使得数据结构的成员能够按某种次序排列
- 提供ES6的for of循环消费

Iterator遍历过程：

- 创建一个指针对象，指向当前数据结构的起始位置；遍历器对象本质就是个指针
- 第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员
- 第二次调用next，指针指向数据结构的第二个成员
- 不断调用指针对象的next方法，直到它指向数据结构的结束位置

ES6规定，默认的Iterator接口部署在数据结构的**Symbol.iterator**属性，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”（iterable）

```javascript
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();

iter.next() // { value: 'a', done: false }
iter.next() // { value: 'b', done: false }
iter.next() // { value: 'c', done: false }
iter.next() // { value: undefined, done: true }
```

调用Iterator接口的场合

- 解构赋值：对数组和Set进行解构赋值时，会默认调用Symbol.iterator方法
- 扩展运算符：扩展运算符（...）也会调用默认的 Iterator 接口
- yield\*：`yield*`后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口
- 其他场合：for of、Array from、Map、Set、Promise.all()、Promise.race()

---

### Promise、Generator

见异步

---

### Class

ES6引入Class，为了使生成对象更接近面向对象语言，让对象原型的写法更加清晰、更像面向对象编程的语法，完全可以看作构造函数的另一种写法

```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
// 等同于
Point.prototype = {
  constructor() {},
  toString() {}
};
```

- 在类的实例上调用方法，其实就是调用原型上的方法
- 由于类的方法都定义在`prototype`对象上面，所以类的新方法可以添加在`prototype`对象上面
- 类.prototype.constructor === 类
- 类内部所有定义的方法，都是不可枚举的，不能通过Object.keys获取，但是原型链上的可以

**constructor**

- 是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法，如果没有显示定义，则会默认添加一个空的constructor
- `constructor`方法默认返回实例对象（即`this`），完全可以指定返回另外一个对象，在其中返回一个新建对象，就会断开原型链

类的实例

- 如果实例的属性不是直接定义在this上，则都是定义在原型链上，hasOwnProperty访问不到
- 与 ES5 一样，类的所有实例共享一个原型对象
- 可以通过实例`__proto__`来为实例添加方法

取值getter和存值setter

- 与 ES5 一样，在“类”的内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为

- ```javascript
  class MyClass {
    constructor() {
      // ...
    }
    get prop() {
      return 'getter';
    }
    set prop(value) {
      console.log('setter: '+value);
    }
  }
  ```

注意点：

- 类和模块的内部，**默认就是严格模式**，所以不需要`use strict`
- 类不同于函数声明，**不存在变量提升**
- name属性总是返回紧跟class关键字后面的类名
- 类中的某个方法之前加`*`，就表示该方法是一个Generator函数
- 类的方法内部this默认执行类的实例，但是如果单独通过解构将方法提出，this则指向undefined
  - 将此方法写在构造方法中，this.fn = this.fn.bind(this)，来绑定this
  - 写在构造方法中，使用箭头函数 this.getThis = () => this
  - Proxy，使用`Proxy`，获取方法的时候，自动绑定`this`

静态方法

- 该类私有的方法，不会被实例继承，在方法前加上static关键字即静态方法
- 静态方法需要直接通过类来调用，实例无法获取该方法
- 静态方法中的this，指向类，而不是实例
- 父类的静态方法，可以被子类继承，也可以在子类super对象上调用

静态属性

- 在class内部，static 属性名 即定义内部静态属性
- 可以直接通过类名访问

私有属性和方法

- class内部，属性、方法名前加上`#`表示该属性私有
- 加了`#`的变量和没加的同名变量是两个变量

new.target属性（new.target 顾名思义就是new后面是谁）

- 该属性一般用在构造函数之中，返回`new`命令作用于的那个构造函数

- 如果构造函数不是通过`new`命令或`Reflect.construct()`调用的，`new.target`会返回`undefined`

- 子类继承父类时，new.target会返回子类，可以利用此特性实现一个必须继承后才能使用的类

  ```javascript
  class Shape {
    constructor() {
      if (new.target === Shape) {
        throw new Error('本类不能实例化');
      }
    }
  }
  
  class Rectangle extends Shape {
    constructor(length, width) {
      super();
      // ...
    }
  }
  
  var x = new Shape();  // 直接父类实例化 报错
  var y = new Rectangle(3, 4);  // 继承后的子类实例化 正确
  ```

  ---

### Class 继承

```javascript
class ColorPoint extends Point {
constructor(x, y, color) {
super(x, y); // 调用父类的constructor(x, y)
this.color = color;
}

toString() {
return this.color + ' ' + super.toString(); // 调用父类的toString()
}
}
```

Class 可以通过`extends`关键字实现继承，子类必须在constructor中调用super方法，否则子类就得不到this

- ES6的继承，实质是先将父类实例对象的属性和方法，加到`this`上面（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`
- ES5 的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）

继承后子类中不显式定义constructor方法的话，子类会自动定义，且在其中调用super

如果在super之前调用this，会报错，因为子类实例的构建基于父类实例，只有`super`方法才能调用父类实例

class继承，同样可以用`Object.getPrototypeOf`方法可以用来从子类上获取父类

**super 关键字**

作为函数使用时：

```javascript
class A {}

class B extends A {
  constructor() {
    super();
  }
}
```

- `super`作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次`super`函数
- `super`虽然代表了父类`A`的构造函数，但是返回的是子类`B`的实例，即`super`内部的`this`指的是`B`的实例，因此`super()`在这里相当于`A.prototype.constructor.call(this)`
- 作为函数时，`super()`只能用在子类的构造函数之中，用在其他地方就会报错

作为对象时：

- `super`作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类（可以看做类似this的一个指针，代表父类）
  - 在子类普通方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类实例
    - 由于`this`指向子类实例，所以如果通过`super`对某个属性赋值，这时`super`就是`this`，赋值的属性会变成子类实例的属性
    - 但是如果直接读super.name，获取的则是父类原型对象上的name属性，返回undefined
  - 在子类的静态方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类，而不是子类的实例
- 由于`super`指向父类的原型对象，所以定义在父类实例上的方法或属性（绑定this），是无法通过`super`调用的

Class的prototype和`__proto__`

相比ES5继承，class继承多一条继承链，即子类的`__proto__`指向对应的构造函数prototype属性

