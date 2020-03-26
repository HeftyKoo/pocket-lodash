# lodash源码分析之reduceRight

本文为读 lodash 源码的第一百五十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import arrayReduceRight from './.internal/arrayReduceRight.js'
import baseEachRight from './.internal/baseEachRight.js'
import baseReduce from './.internal/baseReduce.js'
```

[《lodash源码分析之arrayReduceRight》](internal/arrayReduceRight.md)
[《lodash源码分析之baseEachRight》](internal/baseEachRight.md)
[《lodash源码分析之baseReduce》](internal/baseReduce.md)

## 源码分析

`reduceRight` 和 `reduce` 作用类似，不过是从后向前遍历。

源码如下：

```javascript
function reduceRight(collection, iteratee, accumulator) {
  const func = Array.isArray(collection) ? arrayReduceRight : baseReduce
  const initAccum = arguments.length < 3
  return func(collection, iteratee, accumulator, initAccum, baseEachRight)
}
```

可以看到，和 `reduce` 不一样的地方是，如果是 `array`，则 `func` 为 `arrayReduceRight`，`baseEach` 也用 `baseEachRight` 替代。

其实就是将对应的方法替换成 `xxxRight` 的方法。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 