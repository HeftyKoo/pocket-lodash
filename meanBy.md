# lodash源码分析之meanBy

本文为读 lodash 源码的第二百六十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSum from './.internal/baseSum.js'
```

[《lodash源码分析之baseSum》](internal/baseSum.md)

## 源码分析

`meanBy` 用来计算一组数据的平均值，需要传入 `iteratee`  迭代器。

源码如下：

```javascript
const NAN = 0 / 0
function meanBy(array, iteratee) {
  const length = array == null ? 0 : array.length
  return length ? (baseSum(array, iteratee) / length) : NAN
}
```

如果没有传入数组，或者传入的为空数组，则返回 `NaN` 。

否则，调用 `baseSum` 得出总和，然后再除以 `length` ，即数据的个数，即可得到平均值。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

