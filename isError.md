# lodash源码分析之isError

本文为读 lodash 源码的第二百二十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isObjectLike from './isObjectLike.js'
import isPlainObject from './isPlainObject.js'
```

[《lodash源码分析之getTag》](internal/getTag.md)

[《lodash源码分析之isObjectLike》](isObjectLike.md)

[《lodash源码分析之isPlainObject》](isPlainObject.md)

## 源码分析

`isError` 用来判断一个值是否为 `Error` 对象。

源码如下：

```javascript
function isError(value) {
  if (!isObjectLike(value)) {
    return false
  }
  const tag = getTag(value)
  return tag == '[object Error]' || tag == '[object DOMException]' ||
    (typeof value.message === 'string' && typeof value.name === 'string' && !isPlainObject(value))
}
```

`Error` 肯定是一个对象，因此如果不能通过 `isObjectLike` 检测，返回 `false` 。

接下来检测 `value` 的 `Tag` 是否为 `[object Error]` 或者 `[object DOMException]` 这两种常见的 `Error` 对象，如果是，返回的也是 `true` 。

如果通过 `Tag` 检测不出来，则判断 `message` 和 `name` 是否为 `string` ，因为 `Error` 对象一般都会有这两个属性，并且一般都为 `string` 类型，并且 `value` 不能为纯对象，如果满足这三个条件，也认为是 `Error` 对象，当然，这并不严谨。


## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 