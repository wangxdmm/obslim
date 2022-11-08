# Fork of [observable-slim](https://github.com/elliotnb/observable-slim)

## purpose

For version control of our own low code platform which based on `vue3`

## changes

- batch array changes into one
  - if array operation is oneof `push`, `pop`, `unshift`, `shift`, `splice`, we always want the final array and initial array, unfortunately, `observable-slim` based on `Proxy` always invoke many changs like `array.0`, `array.1`, `array.length` which we don't care anymore, so, I delay pushing change by add it to `Microtasks` by `Promise.resolve`

```javascript
changes.push({
 ,"operation": // 'push', 'pop', 'unshift', 'shift', 'splice'
 ,"type":'unitedArrayChange'
 ,"target":target
 ,"property":undefined
 ,"newValue":target
 ,"previousValue":targetBuffer.value
 ,"currentPath":_getPath(target, property).split('.').slice(0, -1)
 ,"jsonPointer":_getPath(target, property).split('.').slice(0, -1).join('/')
 ,"proxy":proxy
 ,"index": ++index
})
```

- replace `change's path` from `string(a.b.c)` to `array(['a', 'b', 'c'])`
- add `__slim` prefix to all private key added by `observable-slim`, like `__length`, `__isProxy`, `__getPath`
- change some key for our own project to use
  - `operation`
- add `index` flag for sorting changes
- add more `types` for TS

## plan

- refactor all code by typescript
