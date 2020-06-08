# lodash源码分析之hasPath

本文为读 lodash 源码的第三百七十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castPath from './.internal/castPath.js'
import isArguments from './isArguments.js'
import isIndex from './.internal/isIndex.js'
import isLength from './isLength.js'
import toKey from './.internal/toKey.js'
```

[《lodash源码分析之castPath》](./internal/castPath.md)

[《lodash源码分析之isArguments》](./isArguments.md)

[《lodash源码分析之isIndex》](./internal/isIndex.md)

[《lodash源码分析之isLength》](./isLength.md)

[《lodash源码分析之toKey》](./internal/toKey.md)

## 源码分析

`hasPath` 用于检测 `object` 上是否存在路径 `path` 。

如：

```javascript
const object = { 'a': { 'b': 2 } }
hasPath(object, 'a.b') // => true
hasPath(object, ['a', 'b']) // => true
```

源码如下：

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function hasPath(object, path) {
  path = castPath(path, object)

  let index = -1
  let { length } = path
  let result = false
  let key

  while (++index < length) {
    key = toKey(path[index])
    if (!(result = object != null && hasOwnProperty.call(object, key))) {
      break
    }
    object = object[key]
  }
  if (result || ++index != length) {
    return result
  }
  length = object == null ? 0 : object.length
  return !!length && isLength(length) && isIndex(key, length) &&
    (Array.isArray(object) || isArguments(object))
}

```

先是调用 `castPath` 将路径都转换成路径数组。

```javascript
while (++index < length) {
  key = toKey(path[index])
  if (!(result = object != null && hasOwnProperty.call(object, key))) {
    break
  }
  object = object[key]
}
```

接着遍历路径，每次遍历都使用 `toKey` 来确保当前的值为合法的属性名。

可以看到，每次遍历都会将当前的属性值从上层 `object` 上取出，重新赋值给 `object` 。

因此要确保 `key` 为 `object` 的属性，则 `object` 不能为 `null` 和 `undefined` ，如果为这两者，则下一级属性无法取值。

其次，属性 `key` 还要为当前 `object` 自身的属性，即能通过 `Object.hasOwnProperty` 的检测。

如果在遍历的过程中，还满足这两个条件，则结果 `result` 会被置为 `false` ，中止遍历。

```javascript
if (result || ++index != length) {
  return result
}
```

在经过上述的遍历后，会得到结果 `result` ，如果最终的结果 `result` 为 `true` ，则遍历肯定已经完成，中间没有出现过 `result` 为 `false` 的情况，也即属性路径 `path` 在 `object` 上。

如果 `result` 为 `false` ，并且 `++index != length` ，表示属性路径没有遍历完成，也即 `while` 循环中途退出了，那肯定属性路径 `path` 不在 `object` 上，也直接返回结果 `false` 即可。

除了以上两种情况外，还有一种情况，就是遍历完成了，但是 `result` 为 `false` ，这时需要进行下面的判断：

```javascript
length = object == null ? 0 : object.length
return !!length && isLength(length) && isIndex(key, length) &&
  (Array.isArray(object) || isArguments(object))
```

这种是什么情况呢？这种情况发生在稀疏数组，或者 `arguments` 上。

例如：

```javascript
const a = new Array(3)
hasPath(a, '0') // => true
```

在这种情况下，先判断 `object == null` ，如果为 `null` 或者 `undefined` ，则不是数组，则将 `length` 设置为 `0` ，下面可以看到，这种情况是返回 `false` 的。否则从 `object` 上取出 `length` 属性。

先是判断 `!!length` ，如果 `length` 为 `0` ，则数组上没有任何元素，返回 `false` 。

接着判断 `length` 是否为合法的数组长度，如果不是，也返回 `false` 。

接着判断 `key` 是否为 `index` 索引，使用 `isIndex` 来判断，确保 `key` 的值比 `length` 要小。

在上述判断完成后，再判断 `object` 是否为数组或者 `arguments` 对象。

在满足上述条件后，返回 `true` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

