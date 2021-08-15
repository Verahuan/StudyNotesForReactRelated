## 1.前言

[TOC]

```javascript
 A specialized version of `forEach` for arrays.
 //arr.forEach在迭代前不会生成新的副本
```

## arrayEach

```javascript
 /* @private
 * @param {Array} [array] The array to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @returns {Array} Returns `array`.
 */
function arrayEach(array, iteratee) {
  let index = -1
  const length = array.length

  while (++index < length) {
    if (iteratee(array[index], index, array) === false) {
      break
    }
  }
  return array
}

export default arrayEach
```

可以在iteratee中设置提前跳出的条件信号

## arrayEachRight

从右往左开始执行，也可以设置跳出

```javascript
/**
 * A specialized version of `forEachRight` for arrays.
 *
 * @private
 * @param {Array} [array] The array to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @returns {Array} Returns `array`.
 */
function arrayEachRight(array, iteratee) {
  let length = array == null ? 0 : array.length

  while (length--) {
    if (iteratee(array[length], length, array) === false) {
      break
    }
  }
  return array
}

export default arrayEachRight
```

## arrayReduce

```javascript
/**
 * A specialized version of `reduce` for arrays.
 *
 * @private
 * @param {Array} [array] The array to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @param {*} [accumulator] The initial value.
 * @param {boolean} [initAccum] Specify using the first element of `array` as
 *  the initial value.
 * @returns {*} Returns the accumulated value.
 */
function arrayReduce(array, iteratee, accumulator, initAccum) {
  let index = -1
  const length = array == null ? 0 : array.length

  if (initAccum && length) {
    accumulator = c[++index]
  }
    //accumulator初始化为array[0]
    //不传入初始值的化，从index=0开始执行函数
    //length为0直接返回accumulator
  while (++index < length) {
    accumulator = iteratee(accumulator, array[index], index, array)
  }
    //上一次执行的结果accumulator作为下一次的参数传入到回调函数中
  return accumulator
    //返回累计值
}

export default arrayReduce

```

参考MDN手册：

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce

Reduce的使用

Reduce的问题主要在于是否传了初始值，如果没有传初始值，则默认数组的第一个值为初始值，传了初始值，怎使用该初始值

上次函数的返回值作为下一次的accumulator传入，index和当前的数组是为了方便操作，实现复杂功能

```javascript
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

![image-20210222231425792](D:\typora\images\image-20210222231425792.png)

## arrayReduceRight

```javascript
/**
 * A specialized version of `reduceRight` for arrays.
 *
 * @private
 * @param {Array} [array] The array to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @param {*} [accumulator] The initial value.
 * @param {boolean} [initAccum] Specify using the last element of `array` as
 *  the initial value.
 * @returns {*} Returns the accumulated value.
 */
function arrayReduceRight(array, iteratee, accumulator, initAccum) {
  let length = array == null ? 0 : array.length
  if (initAccum && length) {
    accumulator = array[--length]
  }
  while (length--) {
    accumulator = iteratee(accumulator, array[length], length, array)
  }
  return accumulator
}

export default arrayReduceRight

```

## arrayIncludesWith

```javascript
/**
 * This function is like `arrayIncludes` except that it accepts a comparator.
 *
 * @private
 * @param {Array} [array] The array to inspect.
 * @param {*} target The value to search for.
 * @param {Function} comparator The comparator invoked per element.
 * @returns {boolean} Returns `true` if `target` is found, else `false`.
 */
function arrayIncludesWith(array, target, comparator) {
  if (array == null) {
    return false
  }
//功能：将数组中的每个值都与target一起传入 comparator 中，一旦返回true怎说明满足条件
    //comparator 函数是判断两者是否全等
  for (const value of array) {
    if (comparator(target, value)) {
      return true
    }
  }
   //for-in`  一般用来遍历对象，语句以原始插入顺序迭代对象的可枚举属性。`for-in`会把继承链的对象属性都会遍历一遍,所以会更花时间.
  // `for-of` 语句只遍历可迭代对象的数据。
  return false
}

export default arrayIncludesWith

```

