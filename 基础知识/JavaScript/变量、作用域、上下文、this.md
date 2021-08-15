## 目录

[TOC]

### 变量

- 基础类型：存储的为值，占用大小固定，因此保存在栈中
  - 栈是自动分配固定大小的内存空间，并由系统自动释放
- 引用类型：存储的为指向该对象的指针，复制的时候为指针而非值，保存在堆中
  - 堆是动态分配内存，内存大小不一，也不会自动释放

#### 变量提升

- 使用var声明变量，会存在**变量提升**（自动提升到当前作用域最顶层声明）
- **函数声明**：`function fnName() {}`，特点：函数声明提升 => 可以将函数声明放在调用后
  - **函数声明的提升等级高于变量声明**，同名下不会被变量声明覆盖，但会被变量初始化覆盖
- **函数表达式**：`var fnName = function(){}`，创建的函数为匿名函数，name属性为空，只会讲`fnName`当做变量提升，故在定义前执行`fnName`会报错（这种情况就属于变量提升的范围）

### 执行环境（执行上下文）及作用域

- **全局执行环境**是最外围的执行环境
- 每个函数都有自己的**执行环境**。当执行流入一个函数时，函数的环境就会被推入一个环境栈中，执行完毕后推出
- 当代码在一个环境中执行时，会创建变量对象的一个**作用域链**。作用域链的用途是保证对执行环境有权访问的所有变量和函数的有序访问（向上访问）
- 作用域的前端始终是当前执行的代码所在环境的变量对象，末端是全局执行环境

#### 作用域 Scope

- <u>在 JavaScript 中, 作用域（scope，或译有效范围）就是变量和函数的**可访问范围**</u>，即作用域控制着变量和函数的**可见性**和**生命周期**
- `ES5`中只有全局作用域和函数作用域

#### 全局作用域 Global Scope

- 不在任何函数内定义的变量就具有全局作用域
- `JS`默认全局对象window，全局作用域的变量会自动绑定到window上
- `Node.Js`全局对象global，且``Node.Js``中每个`js`文件就是独立完整的作用域（module模块），因此其中var声明的变量即为当前`.js`文件中的局部变量(其实没有学过node,但是全局作用域的时候尝试运行了一下)
  - global全局对象是独立于所有`.js`之上的，想在文件中将对象绑定到global上，直接x=1即可

#### 局部作用域 Local Scope

- `JS`的作用域是通过函数定义，在一个函数中定义的变量只对这个函数内部可见，称为函数（局部）作用域


#### 攀爬作用域链

- 当不同执行上下文之间存在变量名冲突时，可以通过攀爬作用域链解决
- 查找过程总是从自身的变量对象开始

---

### 上下文、作用域

- **上下文和作用域是两个不同的概念**
- 作用域基于函数，上下文基于对象
- 作用域是和每次函数调用时变量的访问有关，且每次独立
- <u>上下文总是关键字this的值，是调用当前可执行代码的对象的引用</u>

---

### 关于this

总体原则，在`ES5`中，this永远指向最后调用它的那个对象，执行时才能确认，定义时不行

当无明确对象时指向widow

**改变this的优先级：new > call/apply/bind > 对象方法 > 函数调用**

#### 普通函数

- 对于直接调用的函数来说，this一定指向window(刚开始也是定义在window上的)
- 严格模式下直接调用指向undefined

#### 对象方法

- 谁调用该方法，this就指向谁
- 具体要看最后执行时，指向的是谁

#### call/apply/bind

- 以apply为例，`fun.apply(thisArg, [argsArray])`
- this指向call/apply/bind的第0个参数，非严格模式指定为null或undefined时会自动指向window
- 当多次调用bind函数时，this永远指向第一次绑定的结果
  
  - bind() 的实现，相当于使用函数在内部包了一个 call / apply ，第二次 bind() 相当于再包住第一次 bind() ,故第二次以后的 bind 是无法生效的
  

代码实现 **(见下)**

#### new方式（构造函数）

- 永远绑定在new生成的实例上，不会被任何改变
  - new的时候内部创建了个对象，将this绑定在上面
- new的时候只有被new出来的`Fn`才是构造函数，它的this才指向new出来的对象，它里面的函数的this还是指向window

#### 箭头函数

- 箭头函数没有自己的`this`、`super`、`arguments`和`new.target`绑定
  **`new.targe`t**日常没有使用到，但是可以参见`MDN`文档，多学学:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target

  突然发现这个软件可以加图标:candy:(太可爱了吧==)

- 箭头函数不能被new调用，this也不可改变，必须通过查找上下文来确定其值

- 如果箭头函数被非箭头函数包含，则`this`绑定的是最近一层非箭头函数的`this`，如果没有上层函数，就是全局对象
  
  - 对象内方法定义箭头函数，指向对象定义的地方 （箭头函数本身不具备this，会向上级作用域寻找，而对象不是作用域）

#### DOM事件处理函数

- `onclick`和`addEventerListener`是指向绑定事件的元素，`this===currentTarget`
- 内联事件处理函数的话，this指向元素本身，如果是内层函数的话，this指向window

---

### 手写call、apply、bind

手写实现的核心即为将调用call/apply/bind的函数作为要绑定的this的方法来调用

#### call

```javascript
Function.prototype.myCall = function(context,...args){
  if(this === Function.prototype) return undefined
  context = context || window
  context.fn = this // 将调用call的fn作为绑定this的对象方法来调用
  const res = context.fn(...args) 
  delete context.fn
  return res
}
```

#### apply

```javascript
// 与call差异只在于处理参数时，call是展开的数组，apply是数组
Function.prototype.myApply = function(context, args){
  if(this === Function.prototype) return undefined
  context.fn = this
  const res = context.fn(...args) 
  delete context.fn
  return res
}
```

#### bind

```javascript
// bind返回的函数，因此需要考虑做构造函数的情况
Function.prototype.myBind = function(context, args1){
  if(this === Function.prototype) return undefined
  const _this = this //外层保存一下this，用箭头函数可以不需要
  return function Fn(args2){
    // 当构造函数时，new会新生成一个对象将this绑定上去
    if(this instanceof Fn) return new _this(...args1, ...args2)
    return _this.apply(context, [...args1, ...args2])
  }
}
```



```javascript
// 由于bind返回的是一个函数，因此需要考虑被作为构造函数的情况
Function.prototype.myBind = function(context, ...args1){
  if(this === Function.prototype) return undefined
  const _this = this
  return function Fn(...args2){
    // 被作为构造函数时，new里面会创建对象绑定this，直接new就完事了
    if(this instanceof Fn) return new _this(...args1, ...args2)
    // 正常函数时与前面无异，可直接调用apply简化
    return _this.apply(context, [...args1,...args2])
  }
}
```

---

### 闭包

闭包就是指有权访问**另一个函数作用域中的变量**的函数，最常见的创建方式就是在函数内部创建另一个函数

通常函数的作用域及所有变量都会在函数执行结束后被销毁。但是在创建一个闭包后，这个函数的作用域就会一直保存到闭包不存在为止，闭包产生的本质就是，当前环境中存在指向父级作用域的引用

```javascript
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12

// 释放对闭包的引用，释放后由垃圾回收机制回收
// 内存不被回收时就会造成内存泄露（内存无法回收）
add5 = null;
add10 = null;
```

#### 闭包for循环问题

闭包中只能取得包含函数中变量的最后一个值，因为闭包所保存的是整个变量对象，而不是某个特殊的变量

```javascript
function createFns(){
    var result = new Array();
    // 保存着作用域链，最后每次向上级寻找，此时i已经全为10
    for(var i=0; i<10; i++){
        result[i] = function(){
            return i;
        };
    };
    return result;
}
// 10个10
```

修改方式一：将绑定i时候的var换成let，let有块级作用域因此无影响

修改方式二：在绑定函数定义时改为立即执行函数
- 函数声明式可以直接加圆括号立即执行并传参，函数定义式不可

```javascript
function createFns(){
    var result = new Array();
    // 每次定义完立即执行，将i传入
    for(var i=0; i<10; i++){
        result[i] = function(num){
            return function(){
                return num;
            };
        }(i);
    };
    return result;
}
```

#### 模拟块级作用域

为了解决变量污染的问题，可模仿块级作用域（立即执行函数：利用函数作用域形成隔离，立即执行销毁污染）

这种做法可以减少闭包的内存占用，因为没有指向匿名函数的引用，当函数执行完毕时，作用域链会立即销毁

```javascript
// 函数声明式
(function(){
    //这里是块级作用域
})()

//函数表达式
var fn = function(){
    //这里是块级作用域
}()
```



#### 闭包的表现形式

- 返回一个函数
- 作为函数参数传递
- 定时器、Ajax请求等回调函数
- `IIFE`，立即执行函数，保存了全局window和当前函数的作用域

主要应用场合：

- 在内存中维持变量：如果缓存数据、柯里化
- 保护函数内的变量安全：模块化

任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数外部访问这些变量。私有变量包括函数的参数、局部变量和函数内定义的其他函数

把有权访问私有变量的公有方法称为**特权方法**

- 在函数内部定义一个方法（闭包），即可访问私有变量

```javascript
function Animal(){
  
  // 私有变量
  var series = "哺乳动物";
  function run(){
    console.log("Run!!!");
  }
  
  // 特权方法
  this.getSeries = function(){
    return series;
  };
}
```

**模块模式（The Module Pattern）**：为单例创建私有变量和方法

**单例（singleton）**：指的是只有一个实例的对象。JavaScript 一般以对象字面量的方式来创建一个单例对象

```javascript
var singleton = {
  name: "percy",
  speak:function(){
    console.log("speaking!!!");
  },
  getName: function(){
    return this.name;
  }
};
```

模块模式创建单例

```javascript
var singleton = function(){
  
  // 私有变量
  var age = 22;
  var speak = function(){
    console.log("speaking!!!");
  };
  
  // 特权（或公有）属性和方法
  return {
    name: "percy",
    getAge: function(){
      return age;
    }
  };
}();
```

通过匿名函数将私有变量及方法包裹，将公有方法暴露出来，模块化

#### 匿名函数

**匿名函数最大的用途是创建闭包**，并且还可以构建命名空间，以减少全局变量的使用。从而使用闭包模块化代码，减少全局变量的污染

```javascript
// 经典匿名函数闭包例子
var countNumber = (function(){
  var num = 0;
  return function(){
    return ++num;
  };
})();
```

#### 闭包的缺点

- 增加内存使用量，且容易造成内存泄漏
- 多查找一层作用域就会减缓一点查找速度

### 闭包扩展：函数柯里化（高阶组件==）

简单来说，柯里化就是只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数

柯里化的主要目的是为了实现参数的复用

#### 手写实现一个`JS`函数柯里化

```javascript
// 判断fn需要的参数是否满足了，没满足递归继续收集参数
function currying(fn, ...args){
  if(args.length >= fn.length) return fn(...args)
  return (...args2) => currying(fn, ...args, ...args2)
}
```

#### 柯里化经典题

```javascript
// 实现一个add方法，使计算结果能够满足如下预期：
add(1)(2)(3) = 6;
add(1, 2, 3)(4) = 10;
add(1)(2)(3)(4)(5) = 15;

function add() {
    // 第一次执行时，定义一个数组专门用来存储所有的参数
    var _args = Array.prototype.slice.call(arguments);

    // 在内部声明一个函数，利用闭包的特性保存_args并收集所有的参数值
    var _adder = function() {
        _args.push(...arguments);
        return _adder;
    };

    // 利用toString隐式转换的特性，当最后执行时隐式转换，并计算最终的值返回
    _adder.toString = function () {
        return _args.reduce(function (a, b) {
            return a + b;
        });
    }
    return _adder;
}

add(1)(2)(3)                // 6
add(1, 2, 3)(4)             // 10
add(1)(2)(3)(4)(5)          // 15
add(2, 6)(1)                // 9
```




---

### 垃圾收集（还需要完善）

内存的生命周期

- 内存分配：局部变量只在函数执行的过程中存在。而在这个过程中，会为局部变量在栈（或堆）内存上分配相应的空间，以便存储它们的值
- 内存使用：然后在函数中使用这些变量，直至函数执行结束
- 内存回收：此时，局部变量就没有存在的必要了，因此可以释放它们的内存以供将来使用

标记清除

- JavaScript 中最常用的垃圾收集方式是 **标记清除**（mark-and-sweep）
- 当变量进入环境（例如，在函数中声明一个变量）时，就将这个变量标记为“进入环境”，而当变量离开环境时，则将其标记为“离开环境”

引用计数

- 另一种不太常见的垃圾收集策略叫做 **引用计数**（reference counting）
- 当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数+1
- 如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数-1
- 当这个值的引用次数变成0时就可以将其占用的内存空间回收回来

---

