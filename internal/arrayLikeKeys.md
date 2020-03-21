# lodash源码分析之arrayLikeKeys

本文为读 lodash 源码的第一百二十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isArguments from '../isArguments.js'
import isBuffer from '../isBuffer.js'
import isIndex from './isIndex.js'
import isTypedArray from '../isTypedArray.js'
```

[《lodash源码分析之isArguments》](../isArguments.md)
[《lodash源码分析之isBuffer》](../isBuffer.md)
[《lodash源码分析之isIndex》](../isIndex.md)
[《lodash源码分析之isTypedArray》](../isTypedArray.md)

## 源码分析

`arrayLikeKeys` 的作用是将 `value` 的 `key` 收集在一个数组中，作为结果返回。如果是 `array`、`arguments`、`buffer`、`typedArray` 等类数组类型，则还会收集索引。

源码如下：

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function arrayLikeKeys(value, inherited) {
  const isArr = Array.isArray(value)
  const isArg = !isArr && isArguments(value)
  const isBuff = !isArr && !isArg && isBuffer(value)
  const isType = !isArr && !isArg && !isBuff && isTypedArray(value)
  const skipIndexes = isArr || isArg || isBuff || isType
  const length = value.length
  const result = new Array(skipIndexes ? length : 0)
  let index = skipIndexes ? -1 : length
  while (++index < length) {
    result[index] = `${index}`
  }
  for (const key in value) {
    if ((inherited || hasOwnProperty.call(value, key)) &&
        !(skipIndexes && (
        // Safari 9 has enumerable `arguments.length` in strict mode.
          (key === 'length' ||
           // Skip index properties.
           isIndex(key, length))
        ))) {
      result.push(key)
    }
  }
  return result
}
```

### 收集索引

`skipIndexes` 用来标识传入的 `value` 是否为 `array`、`arguments`、`buffer`、`typedArray` 等类数组类型，如果是，则还需要收集索引。

首先获取 `value` 的长度，如果 `skipIndexes` 标识为 `true`，则初始化一个长度为 `length` 的数组，用来保存索引。

```javascript
while (++index < length) {
  result[index] = `${index}`
}
```

如果 `skipIndexes` 为 `true`，索引 `index` 的初始值为 `-1`，否则为 `length`，不会进入这个循环，因为使用 `++index`，因此，存入 `result` 的第一个值为 `0`。

可以看到，后面会用 `for...in` 收集 `key`，其实索引也会在 `for...in` 中遍历出来，那为什么还要多此一举，用 `while` 循环来遍历索引呢？

因为像数组会有稀疏数组这种情况，在 `for...in` 中，索引不会全部遍历出来。

### 收集属性

传入的 `value` 可能不是以上类型，或者以上的类型还有其他属性，这些属性也是要收集的。

源码如下：

```javascript
for (const key in value) {
  if ((inherited || hasOwnProperty.call(value, key)) &&
      !(skipIndexes && (
    // Safari 9 has enumerable `arguments.length` in strict mode.
    (key === 'length' ||
     // Skip index properties.
     isIndex(key, length))
  ))) {
    result.push(key)
  }
}
```

使用 `for...in` 来遍历，`for...in`会遍历原型链上的属性，默认只收集 `value` 本身的属性，即属性要通过 `Object.prototype.hasOwnProperty` 的检测，但是可以通过传入标志位 `inherited` 来收集原型链上的属性。

因此收集的属性要满足 `(inherited || hasOwnProperty.call(value, key))` 这个条件。

因为类数组的索引已经收集过，因此在 `for...in` 遍历时需要将索引排除。

条件如下：

```javascript
!(skipIndexes && isIndex(key, length))
```

本来 `arguments` 的 `length` 属性是不枚举，但是在 Safari 9 中，在 `strict mode` 下，`length` 是可以枚举的，因此也要排除。

最终条件如下：

```javascript
!(skipIndexes && ((key === 'length' || isIndex(key, length)))
```

符合条件的数组也存入 `result` 中。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 