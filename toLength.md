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

这要求转换后的值是一个大于 `0` 的整数，并且不会超过 `4294967295`。

之所以是 `4294967295` ，是因为数组的长度要求是一个无符号的32位整数，所以 `MAX_ARRAY_LENGTH` 应为 `Math.pow(2, 32) -1` 即 `4294967295`。

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

如果传入的 `value` 为假值，直接返回 `0` 。

接着使用 `toInteger` 方法，将 `value` 转换成整数。

如果转换后的整数小于 `0` ，也返回 `0` 。

如果转换后的 `value` 大于最大数组长度整数 `MAX_ARRAY_LENGTH`，则返回 `MAX_ARRAY_LENGTH`。

如果在正常的范围内，则返回转换后的整数 `value` 即可。

## 参考资料

[ECMASript](http://ecma-international.org/ecma-262/7.0/#sec-tolength)

[MDN: Array.prototype.length](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Array/length)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 