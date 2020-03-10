# lodash源码分析之baseSet

本文为读 lodash 源码的第一百一十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import assignValue from './assignValue.js'
import castPath from './castPath.js'
import isIndex from './isIndex.js'
import isObject from '../isObject.js'
import toKey from './toKey.js'
```

[《lodash源码分析之assignValue》](./assignValue.md)
[《lodash源码分析之castPath》](./castPath.md)
[《lodash源码分析之isIndex》](./isIndex.md)
[《lodash源码分析之isObject》](../isObject.md)
[《lodash源码分析之toKey》](./toKey.md)

## 源码分析

`baseSet` 接入一个 `object` ，然后将值 `value` 设置到属性路径 `key` 上。

源码如下：

```javascript
function baseSet(object, path, value, customizer) {
  if (!isObject(object)) {
    return object
  }
  path = castPath(path, object)

  const length = path.length
  const lastIndex = length - 1

  let index = -1
  let nested = object

  while (nested != null && ++index < length) {
    const key = toKey(path[index])
    let newValue = value

    if (index != lastIndex) {
      const objValue = nested[key]
      newValue = customizer ? customizer(objValue, key, nested) : undefined
      if (newValue === undefined) {
        newValue = isObject(objValue)
          ? objValue
          : (isIndex(path[index + 1]) ? [] : {})
      }
    }
    assignValue(nested, key, newValue)
    nested = nested[key]
  }
  return object
}
```

### 处理 `object` 不是对象类型的情况

```javascript
if (!isObject(object)) {
  return object
}
```

`object` 参数要通过 `isObject` 的检测，可以是数组、对象和函数等。

如果通不过检测，不需要再走后续的步骤，因为其他类型没办法往上面设置属性。

### 路径转换和几个变量

```javascript
path = castPath(path, object)

const length = path.length
const lastIndex = length - 1

let index = -1
let nested = object
```

使用 `castPath` 将 `path` 转换成路径数组。

用 `length` 保存路径数组 `path` 的长度，方便后续遍历的终止条件判断。

使用 `lastIndex` 保存路径数组最后一个路径的索引，这个变量的作用后面再说。

使用 `index` 来保存当前遍历到的属性的索引值，也作为遍历指示器。

使用 `nested` 保存当前路径层级的值。

### 遍历及设置值

假设传入了这样的对象：

```javascript
const object = {
  a: {
    b: {
      c: 1
    }
  }
}
```

要将路径 `a.b.c` 的值设置为 `2`。

在这种情况下，不需要考虑异常情况，使用源码中的以下代码即可实现：

```javascript
while (++index < length) {
  const key = toKey(path[index])
  let newValue = value

  if (index != lastIndex) {
    const objValue = nested[key]
    newValue = objValue
  }
  assignValue(nested, key, newValue)
  nested = nested[key]
}
```

在遍历 `path` 的过程中，判断当前的索引 `index` 是不是最后的属性，如果不是，则使用 `nexted[key]` 将当前层级的值取出来。例如在第一层 `a` 处，取得的值为 `b:{c: 1}` 。然后使得 `assignValue` 将值重新设置回去，其实在最后一层属性之前，原对象一点变化都没有，就是将当层的值取出，再设置回去。

但是在最后一层时，`newValue` 的值为传入的 `value` ，也就实现了指定路径的值的更新。

### 处理异常情况

同样是 `a.b.c` 的路径，但是传入的对象如下：

```javascript
const object = {}
```

这时，取第一层 `a` 时，取得的值为 `undefined` ，也即当前的 `nexted` 值为 `undefined` ，此时使用 `assignValue` 来赋值到 `undefined` 上，肯定与结果不符。

这时，就需要对每一层都要进行一个判断，如果当前值不能通过 `isObject` 检测，也即没办法往上面设置属性，就需要帮它生成一个空的对象或者数组，以便后续的层级能在上面设置属性。

代码修改如下：

```javascript
while (++index < length) {
  const key = toKey(path[index])
  let newValue = value

  if (index != lastIndex) {
    const objValue = nested[key]
    newValue = isObject(objValue) 
      ? objValue
    	: (isIndex(path[index + 1]) ? [] : {})
  }
  assignValue(nested, key, newValue)
  nested = nested[key]
}
```

使用 `isIndex` 来判断是不是数组的索引类型，如果是，则使用空数组，否则使用空对象。

### `customizer` 支持

`baseSet` 作为内部方法，支持传入 `customizer` 来返回下一个值，不一定是 `isObject` 这样的判断。

如果有传 `customizer` ，则 `newValue` 直接使得 `customizer` 的值。

```javascript
if (index != lastIndex) {
  const objValue = nested[key]
  newValue = customizer ? customizer(objValue, key, nested) : undefined
  if (newValue === undefined) {
    newValue = isObject(objValue)
      ? objValue
    : (isIndex(path[index + 1]) ? [] : {})
  }
}
```

如果 `newValue` 为 `undefined` ，则还是走 `isObject` 的判断。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 