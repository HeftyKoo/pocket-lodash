# lodash源码分析之indexOf

本文为读 lodash 源码的第四十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIndexOf from './.internal/baseIndexOf.js'
```

[《lodash源码分析之baseIndexOf》](internal/baseIndexOf.md)

## 源码分析

`indexOf`  的方法和数组原生的 `indexOf` 方法类似。

```javascript
function indexOf(array, value, fromIndex) {
  const length = array == null ? 0 : array.length
  if (!length) {
    return -1
  }
  let index = fromIndex == null ? 0 : +fromIndex
  if (index < 0) {
    index = Math.max(length + index, 0)
  }
  return baseIndexOf(array, value, index)
}
```

`indexOf` 接收三个参数，第一个 `array` 为数组，第二个参数 `value` 为需要查找的值，第三个 `fromIndex` 参数和数组的 `indexOf` 方法一样，用来指定开始查找的位置。

首先，检测数组的长度，如果数组的长度，如果传进来的数组为 `null` 或者 `undefined` ，或者数组的长度为 `0` ，表示根本不可能存在指定的 `value` 值，直接返回 `-1` 。

然后再对 `fromIndex` 进行一些合规的处理。

如果 `fromIndex` 没有传，则默认从头开始查找，即从 `0` 开始查找，如果有传，则先使用 `+fromIndex` 转换为数字类型。

`fromIndex` 可以为负数，为负数的时候，刚从后往前数，从数到的位置开始查找。如果超出数组的长度，则从 `0` 开始查找。因此使用 `Math.max(length + index, 0)` 来比较两者的最大值，避免超出数组范围。

最后调用 `baseIndexOf` 来查找出指定元素的 `index` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 