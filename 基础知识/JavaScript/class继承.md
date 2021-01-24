## 1.前言

​	学习React过程中，深刻意识到this指向和class继承的重要性，但是无奈已经遗忘了部分，现在进行复习并且整理。

## 2.class的基础介绍

### (1)基础知识

写法：

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
//构造函数的prototype属性，在 ES6 的“类”上面继续存在。事实上，类的所有方法都定义在类的prototype属性上
```

- **构造方法**：constructor()
- `this`关键字：代表实例对象

> 注意，定义`toString()`方法的时候，前面不需要加上`function`这个关键字，直接把函数定义放进去了就可以了。另外，**方法与方法之间不需要逗号分隔，加了会报错**

```javascript
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true
//类本身就指向构造函数
```

### （2）原型指向问题

```javascript
b.constructor === b.__proto__.constructor
//true

```

```javascript
b.constructor === B.prototype.constructor // true
```

## 3.Class的继承

### (1)继承方法

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

子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。

### （2）this指向问题

这是因为子类自己的`this`对象，必须先通过父类的构造函数完成塑造，**得到与父类同样的实例属性和方法**，然后再对其进行加工，加上子类自己的实例属性和方法。**如果不调用`super`方法，子类就得不到`this`对象**。

> ES5 的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）。
>
> ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到`this`上面（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`
>
> ```javascript
> super()`在这里相当于`A.prototype.constructor.call(this)
> //A是父类
> ```

如果子类没有定义`constructor`方法，这个方法会被默认添加，代码如下。也就是说，不管有没有显式定义，任何一个子类都有`constructor`方法

但是，在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字，否则会报错。