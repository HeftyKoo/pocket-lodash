# lodash源码分析之invertBy

本文为读 lodash 源码的第二百九十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`invertBy` 和 `invert` 的作用类似，不过可以传入一个迭代器 `iteratee` ， `iteratee` 接收每次迭代的 `value` ，然后会返回一个新值作为结果的 `key` ，并且 `invertBy` 不像 `invert` ，值只有一个属性，而是有一组属性。

源码如下：

```javascript
const hasOwnProperty = Object.prototype.hasOwnProperty
function invertBy(object, iteratee) {
  const result = {}
  Object.keys(object).forEach((key) => {
    const value = iteratee(object[key])
    if (hasOwnProperty.call(result, value)) {
      result[value].push(key)
    } else {
      result[value] = [key]
    }
  })
  return result
}
```

同样是使用 `Object.keys` 来获取所有的可枚举属性，使用 `forEach` 遍历，在遍历的过程中，调用 `iteratee` 得到 `value` 作为结果的属性。

如果 `result` 中已经存在该属性，则表示在这次迭代之前已经设置过值，则使用 `push` 将这次的属性压入数组中即可。注意这里的 `key` 不会去重。

如果没有，则初始化一个只有 `key` 元素的数组。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 