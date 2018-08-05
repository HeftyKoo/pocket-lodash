# lodash源码分析之isObjectLike

> 这世界上之所以会有无主的东西，方法是因为有人失去了记忆。
>
> ——王小波《万寿寺》

本文为读 lodash 源码的第二十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`isObjectLike` 的源码很短，如下：

```javascript
function isObjectLike(value) {
  return typeof value == 'object' && value !== null
}
```

其实就是使用 `typeof` 操作符，如果返回值为 `object` ，并且值又不为 `null` 时，就认为是类对象。

这里需要简单地说一下 `typeof` 操作符，`typeof` 会遵循下面的规则来返回：

|     类型     |                             结果                             |
| :----------: | :----------------------------------------------------------: |
|  Undefined   |                         'undefined'                          |
|     Null     |                           'object'                           |
|   Boolean    |                          'boolean'                           |
|    Number    |                           'number'                           |
|    String    |                           'string'                           |
|    Symbol    |                           'symbol'                           |
|   宿主对象   | 由宿主实现，但是不能为 `'undefined'`, `'boolean'`, `'number'` 和  `'string'` |
|   函数对象   |                          'function'                          |
| 任意其它对象 |                           'object'                           |

这里需要说一下的是 `null` ，也是 `isObjectLike` 的关键所在，使用 `typeof` 的操作符时，`null` 会返回 `object` ，为什么会这样呢，看 `MDN` 上的解释：

> 在 JavaScript 最初的实现中，JavaScript 中的值是由一个表示类型的标签和实际数据值表示的。对象的类型标签是 0。由于 `null` 代表的是空指针（大多数平台下值为 0x00），因此，null的类型标签也成为了 0，`typeof null`就错误的返回了"`object"`。（[reference](http://www.2ality.com/2013/10/typeof-null.html)）
>
> ECMAScript提出了一个修复（通过opt-in），但[被拒绝](http://wiki.ecmascript.org/doku.php?id=harmony:typeof_null)。这将导致typeof null === 'object'。

另外还有一点需要注意的，在由宿主实现的对象中，规范规定了不能返回 `'undefined'`, `'boolean'`, `'number'` 和  `'string'` 这几种类型，但是 `document.all` 例外，返回的是 `'undefined'` ，这是不遵循规范的实现。

## 参考

* [MDN:typeof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面