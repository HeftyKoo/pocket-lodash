# lodash源码分析之partition

本文为读 lodash 源码的第一百五十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import reduce from './reduce.js'
```
[《lodash源码分析之reduce》](reduce.md)

## 源码分析

`partition` 的作用是将 `collection` 分成两组，将 `predicate` 返回真值的元素归在第一组，其它的归在第二组。

源码如下：

```javascript
function partition(collection, predicate) {
  return reduce(collection, (result, value, key) => (
    result[predicate(value) ? 0 : 1].push(value), result
  ), [[], []])
}
```

使用 `reduce` 遍历，在遍历的过程中，调用 `predicate` 函数，将 `value` 作为参数传入，如果返回真值，则 `push` 进第一组，否则 `push` 进第二组。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 