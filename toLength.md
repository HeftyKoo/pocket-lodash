# lodash源码分析之toLength

本文为读 lodash 源码的第二百四十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import toInteger from './toInteger.js'
```

[《lodash源码分析之toInteger》](toInteger.md)

## 源码分析

`toLength` 是将一个值转换成可以用在类数组中 `length` 属性的值。

这要求转换后的值是一个大于 `0` 的整数，并且不会超过最大的安全整数。

源码如下：

```javascript
const MAX_ARRAY_LENGTH = 4294967295
function toLength(value) {
  if (!value) {
    return 0
  }
  value = toInteger(value)
  if (value < 0) {
    return 0
  }
  if (value > MAX_ARRAY_LENGTH) {
    return MAX_ARRAY_LENGTH
  }
  return value
}
```

最大的安全整数为 `4294967295` 。

如果传入的 `value` 为假值，直接返回 `0` 。

接着使用 `toInteger` 方法，将 `value` 转换成整数。

如果转换后的整数小于 `0` ，也返回 `0` 。

如果转换后的 `value` 大于最大安全整数 `MAX_ARRAY_LENGTH`，则返回最大安全整数。

如果在正常的范围内，则返回转换后的整数 `value` 即可。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 