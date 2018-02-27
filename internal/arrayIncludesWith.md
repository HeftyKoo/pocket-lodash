# lodash源码分析之arrayIncludesWith

> 有思考能力的人靠思考生活，没有思考能力的人靠本能生活，但本能使人坚强，思考却使人软弱。
>
> ——张贤亮《男人的一半是女人》

本文为读 lodash 源码的第十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

```javascript
function arrayIncludesWith(array, target, comparator) {
  if (array == null) {
    return false
  }

  for (const value of array) {
    if (comparator(target, value)) {
      return true
    }
  }
  return false
}
```

`arrayIncludesWith` 的参数比 `arrayIncludes` 的参数要多一个，即 `comparator` 。

`arrayIncludes` 的分析见上篇文章[《lodash源码分析之arrayIncludes》](arrayIncludes.md)。

在 `arrayIncludes` 中，数组中是否存在某项完成由函数接管，而 `arrayIncludesWith` 中，则交由调用者来判断。

首先，如果数组没有传递，会直接返回 `false` 。

接着，遍历数组中的元素，将元素 `value` 和比较值 `target` 作为参数，交由 `comparator` 处理，如果 `comparator` 返回的是真值，则返回 `true` 。

遍历完毕，如果都没有返回真值，则返回 `false` ，表示没有找到。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面