# lodash源码分析之over

本文为读 lodash 源码的第三百五十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import map from './map.js'
```

[《lodash源码分析之map》](./map.md)


## 源码分析

`over` 接收一组函数 `iteratees` ，并返回一个函数，当函数被调用时，会遍历迭代器，并将函数的参数作为迭代器的参数传入，得到一组结果返回。

源码如下：

```javascript
function over(iteratees) {
  return function(...args) {
    return map(iteratees, (iteratee) => iteratee.apply(this, args))
  }
}
```

其实就是调用 `map` 来遍历迭代器，将每个迭代器的结果返回。并且调用器在调用时，`this` 绑定到函数创建时的 `this` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

