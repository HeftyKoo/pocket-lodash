# lodash源码分析之matchesStrictComparable

本文为读 lodash 源码的第三百五十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`matchesStrictComparable` 接收属性 `key` 和值 `srcValue` ，返回一个函数，这个函数接收 `object` 作为参数，并且判断 `object` 上对应的属性 `key` 上的值是否和 `srcValue` 严格相等。

源码如下：

```javascript
function matchesStrictComparable(key, srcValue) {
  return (object) => {
    if (object == null) {
      return false
    }
    return object[key] === srcValue &&
      (srcValue !== undefined || (key in Object(object)))
  }
}
```

在返回的函数中，如果传入的 `object` 为 `null` 或 `undefined` ，则返回 `false` 。

接着再判断 `object[key]` 是否和 `srcValue` 是否相等，注意，这里使用的是 `===` 比较。

在相等后，还有一种情况，传入的值是 `undefined` 时，刚好 `object` 上没有对应的属性 `key` ，这里也能通过 `object[key] === srcValue` 的检测的，所以还需要将这种情况排除。

因此如果 `srcValue !== undefined` ，那 `object[key]` 和 `srcValue` 相等时，肯定也不会是 `undefined` 。

如果 `srcValue` 为 `undefined` ，则判断 `object` 上是否有属性 `key` ，如果有，则认为两者也相等。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

