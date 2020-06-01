# lodash源码分析之cond

本文为读 lodash 源码的第三百四十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import map from './map.js'
```

[《lodash源码分析之map》](./map.md)

## 源码分析

`cond` 接收 `pairs` 参数，并创建一个函数返回。 `pairs` 的形式如下 `[[predicate, func]]` ， `cond` 会迭代 `pairs` ，并且在迭代的过程中调用 `predicate` ，如果 `predicate` 返回 `true` ，则中止迭代，并且调用对应的 `func` ，调用时将所有参数传入。

如：

```javascript
const func = cond([
  [matches({ 'a': 1 }),         () => 'matches A'],
  [conforms({ 'b': isNumber }), () => 'matches B'],
  [() => true,                  () => 'no match']
])
 
 func({ 'a': 1, 'b': 2 })
 // => 'matches A'

 func({ 'a': 0, 'b': 1 })
 // => 'matches B'

 func({ 'a': '1', 'b': '2' })
 // => 'no match'
```

源码如下：

```javascript
function cond(pairs) {
  const length = pairs == null ? 0 : pairs.length

  pairs = !length ? [] : map(pairs, (pair) => {
    if (typeof pair[1] !== 'function') {
      throw new TypeError('Expected a function')
    }
    return [pair[0], pair[1]]
  })

  return (...args) => {
    for (const pair of pairs) {
      if (pair[0].apply(this, args)) {
        return pair[1].apply(this, args)
      }
    }
  }
}
```

在返回函数之前，先对 `pairs` 进行遍历，这个遍历的作用是对 `func` 进行检测，保证其为函数。如果不是函数，则会报错。

接着会返回一个函数，这个函数在调用的时候，会遍历 `pairs` ，并且调用 `predicate` 断言函数，如果断言函数返回真值，则会调用 `func` ，将所有参数传入，并且将结果返回。

`predicate` 和 `func` 的 `this` 都绑定在 `cond` 所创建环境的 `this` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

