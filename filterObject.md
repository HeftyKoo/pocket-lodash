# lodash源码分析之filterObject

本文为读 lodash 源码的第一百五十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`filterObject` 会迭代 `object` 的 `key` 和 `value` ，将 `key` 、`value` 和 `object` 传给 `predicate` 函数，最后返回一个数组，数组包含所有 `predicate` 函数返回真值时的 `value` 值。

源码如下：

```javascript
function filterObject(object, predicate) {
  object = Object(object)
  const result = []

  Object.keys(object).forEach((key) => {
    const value = object[key]
    if (predicate(value, key, object)) {
      result.push(value)
    }
  })
  return result
}
```

先使用 `Object` 构造函数做转换，避免传入的 `object` 为非对象类型。

然后使用 `Object.keys` 获取所有对象自身的 `keys` ，然后使用 `forEach` 遍历，在遍历的过程中，调用 `predicate` 函数，传入 `value`、`key` 和 `object`，如果返回真值，则 `push` 入 `result` 数组中，最后将 `result` 返回。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 