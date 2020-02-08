# lodash源码分析之memoizeCapped

本文为读 lodash 源码的第六十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import memoize from '../memoize.js'
```

[《lodash源码分析之memoize》](../memoize.md)

## 源码分析

`memoizeCapped` 其实调用的是 `memoize` ，不过会定制 `memoize` 的 `resolver` 函数，定制的主要作用是限制缓存的数量，避免缓存太大，占用太多的内存。

```javascript
const MAX_MEMOIZE_SIZE = 500
function memoizeCapped(func) {
  const result = memoize(func, (key) => {
    const { cache } = result
    if (cache.size === MAX_MEMOIZE_SIZE) {
      cache.clear()
    }
    return key
  })

  return result
}
```

设置了缓存最大的数量为 `500`，如果缓存数量超过 `500` ，则会将之前的缓存全部清空。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 