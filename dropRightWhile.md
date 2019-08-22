# lodash源码分析之dropRightWhile

本文为读 lodash 源码的第三十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseWhile from './.internal/baseWhile.js'
```

[lodash源码分析之baseWhile](/internal/baseWhile.md)

## 源码分析

```javascript
function dropRightWhile(array, predicate) {
  return (array != null && array.length)
    ? baseWhile(array, predicate, true, true)
    : []
}
```

先来看看 `dropRightWhile` 的用法：

```javascript
const users = [
  { 'user': 'barney',  'active': false },
  { 'user': 'fred',    'active': true },
  { 'user': 'pebbles', 'active': true }
]
dropRightWhile(users, ({ active }) => active)
```

`dropRightWhile` 内部其实调用的是 `baseWhile` 方法，注意 `baseWhile` 第三个参数 `isDrop` 为 `true`，第四个参数 `fromRight` 也为 `true` 。所以采取的是从右往左遍历，将遍历过的部分移除。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 