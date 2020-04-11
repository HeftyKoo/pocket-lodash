# lodash源码分析之cloneRegExp

本文为读 lodash 源码的第一百八十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`cloneRegExp` 用来复制正则表达式。

源码如下：

```javascript
const reFlags = /\w*$/
function cloneRegExp(regexp) {
  const result = new regexp.constructor(regexp.source, reFlags.exec(regexp))
  result.lastIndex = regexp.lastIndex
  return result
}
```

其实是调用 `new regexp.constructor` 也即 `new RegExp` 来创建一个新的正则表达式。

传入 `constructor` 的参数分别为 `regexp.source` ，也即原来正则表达式的文本，还有 `reFlags.exec(regexp)` ，这个是用来匹配原来正则表达式的模式。这其实相当于 `regexp.flags` ，但是这个属性目前浏览器的支持度还不是很好，只好用这种迂回的方式来匹配。

照理这样就复制好了一个新的正则表达式。但是原来正则表达式的模式中存在有 `g` 这种情况，可能会有问题。

例如以下代码：

```javascript
const reg = /te/g

reg.test('test') // true
console.log(reg.lastIndex) // 2

const copy = cloneRegExp(reg)

reg.test('test') // false
console.log(reg.lastIndex) // 0

copy.test('test') // ?
copy.lastIndex // ?
```

因为 `cloneRegExp` 调用的时机是在 `reg` 第一次调用 `test` 之后，此时 `lastIndex` 已经指向 `2` 的位置。

如果 `cloneRegExp` 不复制 `lastIndex` 的属性，则 `copy.test('test')` 第一次调用时会为 `true` ，并且 `lastIndex` 为 `2` 。

因为复制的时候 `reg` 已经调用了一次 `test` ，此时 `copy` 第一次调用其实相当于 `reg` 的第二次调用，也即结果为 `false` ，并且 `lastIndex` 为 `0` 才和 `reg` 在复制当时的结果一致。

因此在复制的时候也需要将 `lastIndex` 的属性调整。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 