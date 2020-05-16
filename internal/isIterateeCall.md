# lodash源码分析之isIterateeCall

本文为读 lodash 源码的第二百八十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isArrayLike from '../isArrayLike.js'
import isIndex from './isIndex.js'
import isObject from '../isObject.js'
import eq from '../eq.js'
```

[lodash源码分析之isArrayLike](../isArrayLike.md)

[lodash源码分析之isIndex](./isIndex.md)

[lodash源码分析之isObject](../isObject.md)

[lodash源码分析之eq](../eq.md)


## 源码分析

`isIterateeCall` 用来判断传入的 `value` 、 `index` 和 `object` 参数是不是在迭代过程中得到的。

源码如下：

```javascript
function isIterateeCall(value, index, object) {
  if (!isObject(object)) {
    return false
  }
  const type = typeof index
  if (type === 'number'
    ? (isArrayLike(object) && isIndex(index, object.length))
    : (type === 'string' && index in object)
  ) {
    return eq(object[index], value)
  }
  return false
}
```

如果 `object` 通不过 `isObject` 的检测，则这三个参数肯定不是在迭代过程中得到的。

接下来，判断 `index` 的类型，如果 `index` 为数字类型，则迭代的可能是数组，因此接着判断 `object` 能否通过 `isArrayLike` 的检测，如果能通过，再看 `index` 能否通过 `isIndex` 的检测，即 `index` 是否为正常的数组索引。

如果 `index` 和 `object` 的检测都通过了，再判断数组在 `index` 索引上的值是否和 `value` 相等，这些检测都通过后，则参数是迭代中产生的。

如果 `type` 是 `string` 类型，则 `index` 必须要为 `object` 的属性，这个条件满足后，和数组一样，也要判断该属性上的值是否和 `value` 相等。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 