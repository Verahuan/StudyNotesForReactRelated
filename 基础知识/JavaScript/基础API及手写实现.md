## 目录

[TOC]

### 数组API

#### 会改变原数组
- foreach：数组每个元素都执行一次回调函数，无返回
    - array.forEach(function(currentValue, index, arr), thisValue)
- sort：排序，默认(a.b)=> a-b，小的在前
    - array.sort(sortfunction)
- splice：将指定起始位置和个数的元素删除，添加指定新元素
    - array.splice(index,howmany,item1,.....,itemX)

- 

- pop：删除数组的最后一个元素，并返回删除的元素
- shift：删除数组第一个元素，并返回删除的元素
- push：向数组末尾添加一个或多个元素，返回新的长度
    - array.push(item1, item2, ..., itemX)
- unshift：向数组开头添加一个或更多元素，返回新的长度
    - array.unshift(item1,item2, ..., itemX)

- 

- reverse：颠倒数组中元素的顺序
- fill：将一个固定值替换数组的元素
    - array.fill(value, start, end)
    - value：必需。填充的值
    - start：可选。开始填充位置
    - end：可选。停止填充位置 (默认为 array.length)
- copyWithin：从数组的指定位置拷贝元素到数组的另一个指定位置中
    - array.copyWithin(target, start, end)
    - target：必需。复制到指定目标索引位置
    - start：可选。元素复制的起始位置
    - end：可选。停止复制的索引位置 (默认为 array.length)，负数表示倒数

#### 不会改变原数组
- concat：连接数组，返回结果
    - array1.concat(array2,array3,...,arrayX)
- join：将数组元素按指定字符拼接为字符串，默认以`,`间隔
    - array.join(separator)
- slice：将指定起始和终止位置之间的片断切出来返回
    - array.slice(start, end)

- 

- find：返回符合条件的第一个元素
    - array.find(function(currentValue, index, arr),thisValue)
- findIndex：返回符合条件的第一个元素的位置
    - array.findIndex(function(currentValue, index, arr), thisValue)
- indexOf：返回数组中指定元素的第一次出现位置、
    - array.indexOf(item,start)
- lastIndexOf：返回指定元素在数组中最后一次出现位置
    - array.lastIndexOf(item,start)
- includes：判断一个数组是否包含一个指定的值，返回true
    - arr.includes(searchElement, fromIndex)

- 

- map：对所有元素进行指定操作并返回数组
    - array.map(function(currentValue,index,arr), thisValue)
- **reduce：接收一个函数作为累加器，将数组中所有元素按此累加器计算成一个值**
    - array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
- every：判断所有元素是否都符合条件，满足返回true
    - array.every(function(currentValue,index,arr), thisValue)
- filter：返回符合条件的元素
    - array.filter(function(currentValue,index,arr), thisValue)
- some：判断是否至少有一个元素符合条件，返回true
    - array.some(function(currentValue,index,arr),thisValue)

- flat：展开数组，默认展开一层，参数给几展开几层
  - Array.prototype.flat(n)
- flatMap
  - Array.prototype.map(x => x*2) 展开同时执行map

- 

- isArray：判断对象是否为数组，返回 true
    - Array.isArray(obj)
- from：将拥有 length 属性的对象或可迭代的对象转换成数组并返回
    - Array.from(object, mapFunction, thisValue)
- entries：返回数组的可迭代对象
- toString：将数组转换成字符串,拼接=join()
- valueOf：返回 Array 对象的原始值

---
### 字符串API
- charAt：返回指定位置的字符
    - string.charAt(index)
- startsWith：用于检测字符串是否以指定的子字符串开始，返回true
    - string.startsWith(searchvalue, start)
- indexOf：同数组
    - string.indexOf(searchvalue,start)
- lastIndexOf：同数组
    - string.lastIndexOf(searchvalue,start)
- includes：同数组
    - string.includes(searchvalue, start)

- 

- concat：同数组
    - string.concat(string1, string2, ..., stringX)
- slice：同数组
    - string.slice(start,end)
- split：用于把一个字符串分割成字符串数组
    - string.split(separator,limit)
- substr：切取指定位置和长度的字符串返回
    - string.substr(start,length)
- substring：切取指定位置和结束位置之间的字符串，不包括结束位置
    - string.substring(from, to)

- 

- trim：用于删除字符串的头尾空格
- repeat：将字符串复制指定次数并返回
    - string.repeat(count)

- 

- match：在字符串内检索指定的值，或找到一个或多个正则表达式的匹配
    - string.match(regexp)
- replace：在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串，返回replace结果
    - string.replace(searchvalue,newvalue)
- search：用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串，返回该对象的起始位置
    - string.search(searchvalue)

- 

- charCodeAt：返回指定位置的字符的 Unicode 编码
    - string.charCodeAt(index)
- fromCharCode：可接受一个指定的 Unicode 值返回一个字符串
    - String.fromCharCode(n1, n2, ..., nX)

- 

- toLowerCase：将字符串转换为小写并返回
- toUpperCase：将字符串转换为大写并返回
- toLocaleLowerCase：根据本地主机的语言把字符串转换为小写
- toLocaleUpperCase：根据本地主机的语言把字符串转换为大写
- valueOf：可返回 String 对象的原始值
- toString：返回一个表示 String 对象的值

---
### RegExp 正则
#### 组成
var patt=/pattern/modifiers;
- pattern（模式） 描述了表达式的模式
- modifiers(修饰符) 用于指定全局匹配、区分大小写的匹配和多行匹配

#### 修饰符：
- i：大小写不敏感
- g：全局匹配，找出全部匹配项
- m：多行匹配

#### 模式：
- 括号
    -  [a-z]：查找方括号之间的任何字符
    -  [^a-z]：查找任何不在方括号之间的字符
    -  (red|blue|green)：查找任何指定的选项
    -  ()\1：括号指定分组，加上\1代表引用第一个分组
    -  phone.replace(/^(\d{3})\d{4}(\d+)/,"$1****$2")，可用来屏蔽电话号码，$1代表模式里第一个分组
- 元字符
    - .：查找除了换行和行结束符的单个字符
    - \w：a-z、A-Z、0-9，以及 _ (下划线) 字符
    - \W：非单词字符(\w为单词字符)
    - \d：查找数字
    - \D：查找非数字字符
    - \s：查找空白字符
    - \S：查找非空白字符
    - \b：查找位于单词的开头或结尾的匹配
    - \B：匹配非单词边界
    - \n：/\n/匹配换行符
    - \0：查找Null
- 量词
    - n+：匹配包含 ≥1个n 的字符串
    - n*：匹配任何包含 ≥0个n 的字符串
    - n?：匹配任何包含 0/1个 n 的字符串
    - n{X}：匹配包含 X个n 的序列的字符串
    - n{X,}：匹配包含 ≥X个n 的序列的字符串
    - n{X,Y}：匹配包含 ≥X，≤Y个n 的序列的字符串
    - n$：匹配任何结尾为 n 的字符串
    - ^n：匹配任何开头为 n 的字符串
    - ?=n：匹配任何其后紧接指定字符串 n 的字符串
    - ?!n：匹配任何其后没有紧接指定字符串 n 的字符串
- RegExp 对象方法
    - exec：于检索字符串中的正则表达式的匹配，返回匹配值
    - test：用于检测一个字符串是否匹配某个模式，返回true
    - toString：返回正则表达式的字符串值

---

### 对象API

- for (key in obj)：遍历对象键值


#### 获取随机数，要求长度一致
```javascript
var random = Math.random()
random = random + '0000000000'
random = random.slice(0,10)
```

#### 封装一个能遍历数组和对象的forEach函数
```javascript
function forEach(obj, fn){
    if( obj instanceof Array){
        obj.forEach(function(item, index){
            fn(index, item)
        })
    }else{
        var key;
        for(key in obj){
            if(obj.hasOwnProperty(key)){
                fn(key, obj[key])
            }
        }
    }
}
```

---

### 手写实现Array.isArray

```javascript
// 利用Object.prototype.toString.call
Array.myIsArray = function(arr){
  return Object.prototype.toString.call(arr) === "[object Array]"
}
```

---

### 手写实现reduce、map及相互实现

#### 手写reduce

```javascript
// reduce即累加器，返回累加值
// array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
Array.prototype.myReduce = function(callback, initVal){
  let accumulator = initVal ? initVal : this[0]
  // 如果有初值，则从数组第一个参数起，否则从第二个参数起
  for(let i=initVal? 0: 1; i<this.length; i++){
    let _this = this
    accumulator = callback(accumulator, this[i], i, _this)
  }
  return accumulator
}
```

#### 手写map

```javascript
// map即遍历数组，对每个值执行回调函数，返回新数组，注意没这么使用的thisValue
// array.map(function(currentValue,index,arr), thisValue)
Array.prototype.myMap = function(callback, thisArg){
  let arr = []
  for(let i=0; i<this.length; i++){
    arr.push(callback.call(thisArg, this[i], i, this))
  }
  return arr
}
```

#### reduce实现map、filter

```javascript
// map 主要是利用reduce含prev及cur，将prev作为存储结果的数组
Array.prototype.myMap = function(callback, thisArg){
  return this.reduce((prev, cur, index, arr) => {
    prev.push(callback.call(thisArg, cur, index, arr))
    return prev
  },[])
}

// filter相比map主要是判断是否需要推入结果数组
Array.prototype.myFilter = function(callback, thisArg){
  return this.reduce((prev, cur, index, arr) => {
    if(callback.call(thisArg, cur, index, arr)) prev.push(cur)
    return prev
  },[])
}
```

#### map实现reduce

```javascript
// 相比map，reduce多一个prev，将初值和数组拼接，第一个参数赋值给prev即可
Array.prototype.myReduce = function(callback, initVal){
  let prev = 0, newArr = []
  // 是否存在初值，若存在先拼接数组
  newArr = initVal? [initVal,...this]: [...this]
  
  let result = newArr.map((cur, index, arr) => {
    // count即累加器，数组第一位给初值，其他的正常执行即可
    let count = 0
    if(index === 0){
      prev = cur
    }else{
      count = callback(prev, cur, index, arr)
      pre = count
    }
    return count
  })
  // 由于map执行完为数组，最后一位即累加器结果
  return result[result.length-1]
}
```



---

### 手写数组扁平化

#### 手写实现flat

```javascript
// 递归写法
// 默认参数为1
Array.prototype.myFlat = function(num=1){
  // 首先判读调用对象是否为数组
  if(Array.isArray(this)){
    let arr = []
    // num若不为数字或小于0，则直接不展开
    if(!Number(num) || num <= 0){
      return this
    }
    // forEach逐项遍历，若遍历到数组，则展开一层
    this.forEach(item => {
      if(Array.isArray(item)){
        // 由于递归调用，而num--是执行完后才减，但递归会一直保存着进入下一次执行，因此会变成无限展开
        arr = arr.concat(item.myFlat(--num))
      }else{
        arr.push(item)
      }
    })
    return arr
  //非数组直接报错
  }else{
  	throw this + ".flat is not a function"  
  }
}
```

#### reduce实现数组扁平化

```javascript
function flatten(arr) {
  return arr.reduce(prev, curt) => {
    // 递归扁平化
    return prev.concat(Array.isArray(curt)? flatten(curt): curt)
  },[]}
```
#### 迭代实现数组扁平化

```javascript
function flatten(arr){
  while(arr.some((item) => Array.isArray(item))) arr = [].concat(...arr)
  return arr
}
```



---

### 手写数组去重

#### ES6 Set

```javascript
Array.from(new Set(arr))
[...new Set(arr)]
```

#### 遍历数组

```javascript
Array.prototype.distinct = function() {
  let arr = this, res = []
  for(let i=0;  i<arr.length; i++){
    if(res.indexOf(arr[i]) == -1){
      res.push(arr[i])
    }
  }
  return res
}
```

#### 利用filter+indexOf+lastIndexOf

```javascript
Array.prototype.distinct = function(arr) {
  // 利用filter过滤掉非唯一的值
	return arr.filter(item => arr.indexOf(item) === arr.lastIndexOf(item))
}
```

#### 双层循环

```javascript
// 双层循环，外层遍历元素，内层找相同，相同则跳过当前元素，若不同则推入res
Array.prototype.distinct = function() {
  let arr = this, res = []
  for(let i=0;  i<arr.length; i++){
    for(let j=i+1; j<arr.length; j++){
      if(arr[i] === arr[j]){
        // 后面有此元素，当前的可以被跳过了，j再次被拉回i的下一个元素
        j = ++i
      }
    }
    res.push(arr[i])
  }
  return res
}
```

#### splice原地删除

```javascript
Array.prototype.distinct = function() {
  let arr = this, len = arr.length
  for(let i=0;  i<len; i++){
    for(let j=i+1; j<len; j++){
      if(arr[i] === arr[j]){
        arr.splice(j, 1)
        len--
        j--
      }
    }
  }
  return arr
}
```

---

### 数组几个常用方法

#### 合并

```javascript
arr1.concat(arr2,arr3)
[...arr1, ...arr2, ...arr3]
```

#### 复制

```javascript
arr2 = arr1.concat()
arr2 = arr1.slice(0)
arr2 = [...arr1]
[...arr2] = arr1
```

#### 判断元素在数组中是否唯一

```javascript
arr.indexOf(item) !== arr.lastIndexOf(item)
```

#### 类数组对象转化成数组

```javascript
arr = [].slice.call(arrLike)
arr = Array.prototype.slice(arrLike)
arr = Array.from(arrLike)
```

#### 最值

```javascript
Math.max(...arr)
arr.reduce((pre, cur) => Math.max(pre, cur))
```

