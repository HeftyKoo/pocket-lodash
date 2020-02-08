# lodash源码分析之toKey

本文为读 lodash 源码的第七十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isSymbol from '../isSymbol.js'
```

[《lodash源码分析之isSymbol》](../isSymbol.md)

## 源码分析

`toKey` 是 `lodash` 的内部函数，作用是将一个值转换成符合符合对象的属性。这个和 `js` 的自动转换有一点不同，`js` 自动转换时，会将数字 `-0` 转换成字符串 `"0"`，但是 `toKey` 会将数字 `-0` 转换成字符串 `"-0"`。

源码如下：

```javascript
const INFINITY = 1 / 0
function toKey(value) {
  if (typeof value === 'string' || isSymbol(value)) {
    return value
  }
  const result = `${value}`
  return (result == '0' && (1 / value) == -INFINITY) ? '-0' : result
}
```

首先判断 `value` 是否为字符串或者 `symbol` 类型，如果为这两个类型，都是对象的属性值，直接返回。

如果不是这两种类型，需要将 `value` 转换成字符串。

在数字转换成字符串时， `+0` 和 `-0` 都会被转换成 `"0"` ，因此，判断 `result` 如果为 `"0"` 时，再使用 `1/value` ，看得到的结果是不是 `-INFINITY` ，如果为负无穷，则 `value` 可以判断出来为 `-0` ，直接返回 `"-0"` ，否则返回转换后的字符串 `result` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 