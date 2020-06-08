# lodash源码分析之someValues

本文为读 lodash 源码的第三百七十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`someValues` 和 `some` 的作用类似，不过 `some` 遍历的是数组， `someValues` 针对的是对象。

`someValues` 接收一个对象 `object` 和一个断言函数 `predicate` ，如果对象中只要有一个值能通过 `predicate` 的检测，则得到结果 `true` ，如果所有的值都通不过 `predicate` 检测，则得到 `false` 。

源码如下：

```javascript
function someValues(object, predicate) {
  object = Object(object)
  const props = Object.keys(object)

  for (const key of props) {
    if (predicate(object[key], key, object)) {
      return true
    }
  }
  return false
}
```

先使用 `Object` 将传入的 `object` 进行转换，避免传入非 `object` 的情况。

使用 `Object.keys` 获取 `object` 上所有可枚举属性。

接着遍历属性，在遍历的过程中调用 `predicate` 函数，将值 `object[key]` 、对应的属性 `key` 和 `object` 传入，如果 `predicate` 返回的是真值，则中止遍历，得到结果 `true` 。

如果遍历完毕，`predicate` 得到的都是假值，则得到结果 `false` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

