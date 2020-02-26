# lodash源码分析之takeRight

本文为读 lodash 源码的第九十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import slice from './slice.js'
```

[《读lodash源码之从slice看稀疏数组与密集数组》](slice.md)

## 源码分析

`takeRight` 的作用跟 `take` 差不多，只不过 `takeRight` 是截取数组后面的 `n` 个元素作为新的数组返回。

源码如下：

```javascript
function takeRight(array, n=1) {
  const length = array == null ? 0 : array.length
  if (!length) {
    return []
  }
  n = length - n
  return slice(array, n < 0 ? 0 : n, length)
}

```

因为是从后截取，所以 `slice` 的第三个参数  `end` 很容易就确定为 `length`，即数组的长度。

那第二个参数 `start` 是什么呢？因为是截取 `n` 个元素，所以 `start` 也很容易知道为 `length - n` 。

此时还要处理 `length - n` 小于 `0` 的情况，这种情况下，截取的元素数量超出是 `array` 的范围，在这种情况下，只能将 `array` 的所有元素都返回，也即 `start` 为 `0` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 