# lodash源码分析之fromPairs

本文为读 lodash 源码的第四十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 使用

`fromPairs` 的作用是将类似于 `[['a',1],['b',2]]` 这样的二维数组转换为对象：`{a: 1, b: 2}`。

使用方式如下：

```javascript
fromPairs([['a',1],['b',2]]) // {a: 1, b: 2}
```

## 源码分析

源码如下：

```javascript
function fromPairs(pairs) {
  const result = {}
  if (pairs == null) {
    return result
  }
  for (const pair of pairs) {
    result[pair[0]] = pair[1]
  }
  return result
}
```

首先，使用一个空对象 `result` 作为容器，如果没有传递 `pairs` 参数，则返回空对象。

接着就遍历 `paris` 二维数组，每次遍历，会得到类似 `['a', 1]` 形式的数组，会以该数组的第一项作为对象的 `key`，以第二项作为 `value` 。

最后将 `result` 返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 