# lodash源码分析之isNative

本文为读 lodash 源码的第二百三十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isObject from './isObject.js'
```

[《lodash源码分析之isObject》](./isObject.md)

## 源码分析

`isNatve` 用来检测 `value` 是否为原生内置函数。

源码如下：

```javascript
const reRegExpChar = /[\\^$.*+?()[\]{}|]/g

const reIsNative = RegExp(`^${
  Function.prototype.toString.call(Object.prototype.hasOwnProperty)
    .replace(reRegExpChar, '\\$&')
    .replace(/hasOwnProperty|(function).*?(?=\\\()| for .+?(?=\\\])/g, '$1.*?')
}$`)

function isNative(value) {
  return isObject(value) && reIsNative.test(value)
}
```

首先判断 `value` 是否为对象，再使用 `reIsNative` 来检测是否为原生内置的函数。

这里有几个点很有意思，首先使用 `reIsNative` 时，会隐式将 `value` 转换成 `string` ，如果 `value` 是函数的话，相当于调用了 `toString` 方法。

然后，`reIsNative` 是根据所处的执行环境，动态生成的正则表达式。

```javascript
Function.prototype.toString.call(Object.prototype.hasOwnProperty)
```

`Object.prototype.hasOwnProperty` 只要不做原型链方法的修改，就是一个原生函数，通过这样可以获得原生函数转换成字符串后的模板，然后根据这个模板生成正则。

为什么要通过模板来生成正则，而不是直接写出一个正则表达式呢？

这里因为，在不同的环境下，原生函数 `toString` 后的表现是不一样的。

如在某些浏览器中，`toString` 后是这样的：

```javascript
"function hasOwnProperty() { [native code] }"
```

只有一行。

在某些浏览器中，`toString` 后是这样的：

```javascript
`function hasOwnProperty() {
    [native code]
}`
```

会有缩进和换行。

因此 `lodash` 一开始就通过一个原生函数来获取模板，然后将 `name` 部分替换。

`reRegExpChar` 会将 `(){}[]` 等特殊字符转义。

而 `/hasOwnProperty|(function).*?(?=\\\()| for .+?(?=\\\])/g` 这个正则，在正常情况下，只需要 `/hasOwnProperty|(function).*?(?=\\\()/g` 这样即可，用来匹配函数名。但是某些平台如 `Rhino` 会使用 `for...` 添加额外的信息。

## 参考

[检测一个函数是否是JavaScript原生函数的小技巧](http://www.uxys.com/html/JavaScript/20150313/38023.html)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 