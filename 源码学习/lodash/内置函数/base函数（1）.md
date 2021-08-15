## 1.前言

[TOC]

## baseAssignValue

```javascript
/**
 * The base implementation of `assignValue` and `assignMergeValue` without
 * value checks.
 *
 * @private
 * @param {Object} object The object to modify.
 * @param {string} key The key of the property to assign.
 * @param {*} value The value to assign.
 */
function baseAssignValue(object, key, value) {
  if (key == '__proto__') {
    Object.defineProperty(object, key, {
      'configurable': true,
      'enumerable': true,
      'value': value,
      'writable': true
    })
      //这么写的原因是直接给'__proto__'赋值不会生效
  } else {
    object[key] = value
  }
}

export default baseAssignValue

```

## baseConformsTo

```javascript
/**
 * The base implementation of `conformsTo` which accepts `props` to check.
 *
 * @private
 * @param {Object} object The object to inspect.
 * @param {Object} source The object of property predicates to conform to.
 * @returns {boolean} Returns `true` if `object` conforms, else `false`.
 */
function baseConformsTo(object, source, props) {
  let length = props.length
  if (object == null) {
    return !length
  }
  object = Object(object)
  while (length--) {
    const key = props[length]
    const predicate = source[key]
    const value = object[key]

    if ((value === undefined && !(key in object)) || !predicate(value)) {
      return false
    }
  }
  return true
}

export default baseConformsTo

```

传入obj用对象进行包裹的作用：

//实际上并不是 undefined==null 已经包含了这种情况

![image-20210223235733829](D:\typora\images\image-20210223235733829.png)

## baseFindIndex

```javascript
/**
 * The base implementation of `findIndex` and `findLastIndex`.
 *
 * @private
 * @param {Array} array The array to inspect.
 * @param {Function} predicate The function invoked per iteration.
 * @param {number} fromIndex The index to search from.
 * @param {boolean} [fromRight] Specify iterating from right to left.
 * @returns {number} Returns the index of the matched value, else `-1`.
 */
function baseFindIndex(array, predicate, fromIndex, fromRight) {
  const { length } = array //解构写法要时常使用
  let index = fromIndex + (fromRight ? 1 : -1)
  //从fromindex的左边开始 还是右边开始（fromRight==true）

  while ((fromRight ? index-- : ++index < length)) {
      //:的优先级大于<
    if (predicate(array[index], index, array)) {
      return index
    }
      //返回满足条件的序号
  }
  return -1
}

export default baseFindIndex
```

## baseFindKey

// 数组中满足predicate的值对应的key

```javascript
/**
 * The base implementation of methods like `findKey` and `findLastKey`
 * which iterates over `collection` using `eachFunc`.
 *
 * @private
 * @param {Array|Object} collection The collection to inspect.
 * @param {Function} predicate The function invoked per iteration.
 * @param {Function} eachFunc The function to iterate over `collection`.
 * @returns {*} Returns the found element or its key, else `undefined`.
 */
function baseFindKey(collection, predicate, eachFunc) {
  let result
  eachFunc(collection, (value, key, collection) => {
    if (predicate(value, key, collection)) {
      result = key
      return false
    }
      //eachFunc的设置应该为当第二个参数函数返回值为false的时候，不再接着执行
  })
  return result
}

export default baseFindKey
```

## baseFor

```javascript
/**
 * The base implementation of `baseForOwn` which iterates over `object`
 * properties returned by `keysFunc` and invokes `iteratee` for each property.
 * Iteratee functions may exit iteration early by explicitly returning `false`.
 *
 * @private
 * @param {Object} object The object to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @param {Function} keysFunc The function to get the keys of `object`.
 * @returns {Object} Returns `object`.
 */
function baseFor(object, iteratee, keysFunc) {
  const iterable = Object(object)
  const props = keysFunc(object) //获得key
  let { length } = props
  let index = -1

  while (length--) {
    const key = props[++index]
    if (iteratee(iterable[key], key, iterable) === false) {
      break //遇到不满足每次遍历执行的函数（iteratee）的元素时，直接停止执行
    }
  }
  return object
}

export default baseFor
```

## baseForRight

//和上面的区别在于变成从右到左执行

```javascript
/**
 * This function is like `baseFor` except that it iterates over properties
 * in the opposite order.
 *
 * @private
 * @param {Object} object The object to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @param {Function} keysFunc The function to get the keys of `object`.
 * @returns {Object} Returns `object`.
 */
function baseForRight(object, iteratee, keysFunc) {
  const iterable = Object(object)
  const props = keysFunc(object)
  let { length } = props

  while (length--) {
    const key = props[length]
    if (iteratee(iterable[key], key, iterable) === false) {
      break
    }
  }
  return object
}

export default baseForRight
```

## baseIndexOfWith

```javascript
/**
 * This function is like `baseIndexOf` except that it accepts a comparator.
 *
 * @private
 * @param {Array} array The array to inspect.
 * @param {*} value The value to search for.
 * @param {number} fromIndex The index to search from.
 * @param {Function} comparator The comparator invoked per element.
 * @returns {number} Returns the index of the matched value, else `-1`.
 */
function baseIndexOfWith(array, value, fromIndex, comparator) {
  let index = fromIndex - 1 //从第几个位置开始
  const { length } = array

  while (++index < length) {
    if (comparator(array[index], value)) {
      return index
    }
      //返回数组中第一个满足comparator(array[index], value)的值的index
      //否则返回-1
      //类似：indexOf LastIndexOf
  }
  return -1
}

export default baseIndexOfWith
```

## baseInRange

```javascript
/**
 * The base implementation of `inRange` which doesn't coerce arguments.
 *
 * @private
 * @param {number} number The number to check.
 * @param {number} start The start of the range.
 * @param {number} end The end of the range.
 * @returns {boolean} Returns `true` if `number` is in the range, else `false`.
 */
function baseInRange(number, start, end) {
  return number >= Math.min(start, end) && number < Math.max(start, end)
}
//判断传入number是不是处在start和end之间

export default baseInRange
```

## baseIsNaN

 isNaN

区分：NaN和null不一样

```javascript
/**
 * The base implementation of `isNaN` without support for number objects.
 *
 * @private
 * @param {*} value The value to check.
 * @returns {boolean} Returns `true` if `value` is `NaN`, else `false`.
 */
function baseIsNaN(value) {
  return value !== value
}
// isNaN 
/*
区分：NaN和null不一样
null!==null
false
NaN!==NaN
true
*/
export default baseIsNaN
```

## baseProperty

```javascript
/**
 * The base implementation of `property` without support for deep paths.
 *
 * @private
 * @param {string} key The key of the property to get.
 * @returns {Function} Returns the new accessor function.
 */
function baseProperty(key) {
  return (object) => object == null ? undefined : object[key]
}
//问题来了 object还没有哇，上文有吧==等待看应用
export default baseProperty
```

## basePropertyOf

```javascript
/**
 * The base implementation of `propertyOf` without support for deep paths.
 *
 * @private
 * @param {Object} object The object to query.
 * @returns {Function} Returns the new accessor function.
 */
function basePropertyOf(object) {
  return (key) => object == null ? undefined : object[key]
}

export default basePropertyOf

```

## baseRange

生成一个数组，改数组由指定的数开始，指定的数结束，并且，每个数组元素之间的差值会指定

```javascript
/**
 * The base implementation of `range` and `rangeRight` which doesn't
 * coerce arguments.
 *
 * @private
 * @param {number} start The start of the range.
 * @param {number} end The end of the range.
 * @param {number} step The value to increment or decrement by.
 * @param {boolean} [fromRight] Specify iterating from right to left.
 * @returns {Array} Returns the range of numbers.
 */
function baseRange(start, end, step, fromRight) {
  let index = -1
  let length = Math.max(Math.ceil((end - start) / (step || 1)), 0)//判断循环次数 ceil向上取整
  const result = new Array(length)

  while (length--) {
    result[fromRight ? length : ++index] = start
    start += step
  }
  return result
}

export default baseRange
```

## baseReduce

```javascript
/**
 * The base implementation of `reduce` and `reduceRight` which iterates
 * over `collection` using `eachFunc`.
 *
 * @private
 * @param {Array|Object} collection The collection to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @param {*} accumulator The initial value.
 * @param {boolean} initAccum Specify using the first or last element of
 *  `collection` as the initial value.
 * @param {Function} eachFunc The function to iterate over `collection`.
 * @returns {*} Returns the accumulated value.
 */
function baseReduce(collection, iteratee, accumulator, initAccum, eachFunc) {
  eachFunc(collection, (value, index, collection) => {
    accumulator = initAccum
      ? (initAccum = false, value)
      : iteratee(accumulator, value, index, collection)
  })
  return accumulator
}

export default baseReduce


```

## baseReduce

```javascript
/**
 * The base implementation of `reduce` and `reduceRight` which iterates
 * over `collection` using `eachFunc`.
 *
 * @private
 * @param {Array|Object} collection The collection to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @param {*} accumulator The initial value.
 * @param {boolean} initAccum Specify using the first or last element of
 *  `collection` as the initial value.
 * @param {Function} eachFunc The function to iterate over `collection`.
 * @returns {*} Returns the accumulated value.
 */
function baseReduce(collection, iteratee, accumulator, initAccum, eachFunc) {
  eachFunc(collection, (value, index, collection) => {
    accumulator = initAccum
      ? (initAccum = false, value) //这部分会执行，并且，value会赋值给accumulator
      : iteratee(accumulator, value, index, collection)
  })
  return accumulator
}

export default baseReduce

```

![image-20210309232354186](D:\typora\images\image-20210309232354186.png)

## baseSortBy

```javascript
/**
 * The base implementation of `sortBy` which uses `comparer` to define the
 * sort order of `array` and replaces criteria objects with their corresponding
 * values.
 *
 * @private
 * @param {Array} array The array to sort.
 * @param {Function} comparer The function to define sort order.
 * @returns {Array} Returns `array`.
 */
function baseSortBy(array, comparer) {
  let { length } = array

  array.sort(comparer)
  while (length--) {
    array[length] = array[length].value//把对象替换为其对应的值
  }
  return array
}

export default baseSortBy

```

## baseSum

返回 数组中经过iteratee处理之后的值的和

```javascript
/**
 * The base implementation of `sum` and `sumBy`.
 *
 * @private
 * @param {Array} array The array to iterate over.
 * @param {Function} iteratee The function invoked per iteration.
 * @returns {number} Returns the sum.
 */
function baseSum(array, iteratee) {
  let result

  for (const value of array) {
    const current = iteratee(value)
    //加的是经过iteratee处理之后的值
    if (current !== undefined) {
      result = result === undefined ? current : (result + current)
    }
  }
  return result
}

export default baseSum

```

