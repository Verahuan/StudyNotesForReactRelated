源码

意义何在？

## addMapEntry

```javascript
function addMapEntry(map, pair) {
  // Don't return `map.set` because it's not chainable in IE 11.
  map.set(pair[0], pair[1])
  return map
}

export default addMapEntry
```

## addSetEntry

```javascript
function addSetEntry(set, value) {
  // Don't return `set.add` because it's not chainable in IE 11.
  set.add(value)
  return set
}
```

