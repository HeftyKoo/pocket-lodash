# lodash源码分析之findKey

本文为读 lodash 源码的第二百八十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)


## 源码分析

`findKey` 可以传入一个 `object` 和断言函数 `predicate` ，这个函数的作用跟 `find` 参不多，只不过 `find` 返回的是第一个符合条件的 `value` ，但是 `findkey` 返回的是第一个符合条件的属性 `key` 。

源码如下：
```javascript
function findKey(object, predicate) {
  let result
  if (object == null) {
    return result
  }
  Object.keys(object).some((key) => {
    const value = object[key]
    if (predicate(value, key, object)) {
      result = key
      return true
    }
  })
  return result
}

```

如果 `object` 为 `null` 或者没有传入，返回 `undefined` 。

接着使用 `Object.keys` 来获取 `object` 上的可枚举属性，使用 `some` 来遍历属性。

使用 `some` 的好处是，找到第一个符合条件的属性时就可以终止迭代。

在迭代的过程中，会调用 `predicate` 函数 ，将 `value` 和 `key` 及原始对象 `object`  传入，如果 `predicate` 函数返回真值时，会停止迭代，并且将当前的属性 `key` 赋值给 `result` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 