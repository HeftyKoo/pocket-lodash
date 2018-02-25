# lodash源码分析之arrayIncludes

> 找到一样你所钟爱的事物，然后，永不放手。
>
> ——小熊维尼

本文为读 lodash 源码的第十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIndexOf from './baseIndexOf.js'
```

[《lodash源码分析之baseIndexOf》](baseIndexOf.md)

## 源码分析

```javascript
function arrayIncludes(array, value) {
  const length = array == null ? 0 : array.length
  return !!length && baseIndexOf(array, value, 0) > -1
}
```

`arrayIncludes` 的作用类似于数组的 `includes` 方法，如果查找的元素存在于数组中，则返回 `true` ，不存在返回 `false` 。

`arrayIncludes` 与 `includes` 方法的区别在于 `arrayIncludes` 函数不能指定数组从何处开始查找，而 `includes` 方法则允许指定开始查找的位置。

`length` 为数组的长度，如果数组为 `null` 或者 `undefined` ，则默认 `length` 为 `0` 。

如果 `length` 为 `0` ，则返回 `false` ，否则调用 `baseIndexOf` 查找元素 `value` 在数组中的索引，如果索引大于 `-1` ，表示元素在数组中存在，返回 `true` ，否则返回 `false` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面