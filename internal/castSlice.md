# lodash源码分析之castSlice

本文为读 lodash 源码的第三百零九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import slice from '../slice.js'
```

[lodash源码分析之slice](../slice.md)


## 源码分析

`castSlice` 会在必要的时候，调用 `slice` 来截取数组，返回一个新数组，如果不必要时，返回的是原数组。

源码如下：

```javascript
function castSlice(array, start, end) {
  const { length } = array
  end = end === undefined ? length : end
  return (!start && end >= length) ? array : slice(array, start, end)
}
```

如果没有传入 `end` ，则将 `end` 默认为数组的长度 `length` 。

可以看到，如果没有传入 `start` 为假值，并且 `end` 大于或等于数组长度时，返回的是原数组，否则调用 `slice` 来截取数组。

其实可以理解为，在调用 `slice` 得到和原数组一样的副本时，直接返回原数组，不调用 `slice` 得到副本，这个函数主要出于性能考虑，避免不必要的遍历。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 