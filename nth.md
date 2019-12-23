# lodash源码分析之nth

本文为读 lodash 源码的第五十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isIndex from './.internal/isIndex.js'
```

[lodash源码分析之isIndex](internal/isIndex.md)

## 源码分析

`nth` 用来获取数组中指定索引的值，如果指定的索引为负数，则索引值是从后往前数。

```javascript
function nth(array, n) {
  const length = array == null ? 0 : array.length
  if (!length) {
    return
  }
  n += n < 0 ? length : 0
  return isIndex(n, length) ? array[n] : undefined
}
```

首先获取数组的长度，如果没有传入数组，则默认 `length` 是 `0` ，如果 `length` 是 `0` ，表示没有元素，则返回 `undefined` 。

```javascript
n += n < 0 ? length : 0
```

然后判断要获取的索引是否在 `length` 的范围内，如果在数组最大长度的范围内，则使用 `array[n]` 获取对应索引的值，否则返回 `undefined`。

可以看到 `nth` 的核心就是 `array[n]`，只不过它支持负数的索引，也对索引值的合法性做了校验。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 