# lodash源码分析之cacheHas

> 一个民族，不能只纪念一个人，否则它就被自我轻视。
>
> ——熊培云《思想国》

本文为读 lodash 源码的第十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

本文的源码相当简单，本没必要单独成篇，但是考虑到一致性，还是用单独的篇幅介绍。

## 源码分析

```javascript
function cacheHas(cache, key) {
  return cache.has(key)
}
```

这个方法用来判断指定的 `key` 是否已经存在于缓存中。

如果有看《[lodash源码分析之Hash缓存](Hash.md)》、《[lodash源码分析之List缓存](ListCache.md)》、《[lodash源码分析之缓存方式的选择](MapCache.md)》和 《[lodash源码分析之缓存使用方式的进一步封装](SetCache.md)》这几篇文章，应该会注意到，它们都提供了 `has` 的方法来判断指定的值是否已经在缓存中存在。

其实通过调用缓存实例的 `has` 方法已经足够简单，为什么还要一个 `cacheHas` 的函数呢？我猜可能是不够函数式，缓存实例可能会赋值给不同的命名变量，代码读起来不够直观，下一篇文章会看到 `cacheHas` 的优势。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面