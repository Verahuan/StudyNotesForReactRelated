## 1.前言

简单的字符串操作

[TOC]

## `asciiSize`

解构

```javascript
/**
 * Gets the size of an ASCII `string`.
 *
 * @private
 * @param {string} string The string inspect.
 * @returns {number} Returns the string size.
 */
function asciiSize({ length }) {
  return length
}
//这里主要是一个解构，可以直接返回带有length属性的长度
export default asciiSize
```

## `asciiToArray`

字符串的内置函数split

```javascript
/**
 * Converts an ASCII `string` to an array.
 *
 * @private
 * @param {string} string The string to convert.
 * @returns {Array} Returns the converted array.
 */
function asciiToArray(string) {
  return string.split('')
}

export default asciiToArray
```

