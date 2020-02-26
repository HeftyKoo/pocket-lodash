# lodash源码分析之take

本文为读 lodash 源码的第九十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import slice from './slice.js'
```

[《读lodash源码之从slice看稀疏数组与密集数组》](slice.md)

## 源码分析

`take` 的作用是，从指定的数组 `array` 中，从开始位置，取出 `n` 个元素，组成新的数组作为结果返回。如果没有指定 `n` ，`n` 默认为 `1` 。

源码如下：

```javascript
function take(array, n=1) {
  if (!(array != null && array.length)) {
    return []
  }
  return slice(array, 0, n < 0 ? 0 : n)
}
```

惯例，如果数组没有传入，或者传入的数组长度为 `0` ，直接返回空数组。

`take` 的实现也很简单，调用 `slice(array, 0, n)` 即可。

其实 `slice` 支持负数的，但是 `take` 中 `n` 的含义是元素的个数，不可能为负数，因为判断 `n < 0` 时，传给 `slice` 的第三个参数为 `0` ，这时返回的是空数组。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 