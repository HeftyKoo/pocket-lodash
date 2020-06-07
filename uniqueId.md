# lodash源码分析之uniqueId

本文为读 lodash 源码的第三百七十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`uniqueId` 用来生成唯一 ID，如果有传入 `prefix` ，则生成的 ID 前面会拼接上 `prefix` 。

源码如下：

```javascript
const idCounter = {}
function uniqueId(prefix='$lodash$') {
  if (!idCounter[prefix]) {
    idCounter[prefix] = 0
  }

  const id =++idCounter[prefix]
  if (prefix === '$lodash$') {
    return `${id}`
  }

  return `${prefix + id}`
}

export default uniqueId
```

使用 `idCounter` 来记录已经生成的 ID，不同的 `prefix` 都有独自的 ID 计数器， `prefix` 默认为 `$lodash$` ，但是默认的 `prefix` 不会拼接在生成的 ID 前面。

调用函数时，首先先判断 `idCounter[prefix]` 下是否已经生成过 ID，如果没有，则先初始化为 `0` 。

接下来自增计数器，得到 `id` ，可以看到，第一次调用时，生成的 `id` 为 `1` 。

如果 `prefix` 为 `$loadsh$` ，则认为没有传入 `prefix` ，直接返回 `id` 字符串。

如果有传入 `prefix` ，则将 `prefix` 拼接在 `id` 前面。  

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

