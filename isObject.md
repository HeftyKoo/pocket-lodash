# lodash源码分析之isObject

本文为读 lodash 源码的第五十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`isObject` 用来判断一个值是否为 `object` 类型。

这个方法最主要的一点就是排除了 `null` ，看下源码的实现：

```javascript
function isObject(value) {
  const type = typeof value
  return value != null && (type === 'object' || type === 'function')
}
```

可以看到，`isObject` 是基于 `typeof` 来实现的，`typeof null` 会返回 `object`， 因此需要将 `null` 排除掉。

另外 `function` 其实也是 `object` 类型，但是用 `typeof`来判断时，会返回 `function` ，所以也要对函数进行特殊的处理。

## 相关链接

[ECMA规范](http://www.ecma-international.org/ecma-262/7.0/#sec-ecmascript-language-types)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 