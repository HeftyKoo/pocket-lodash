# lodash源码分析之isElement

本文为读 lodash 源码的第二百一十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isObjectLike from './isObjectLike.js'
import isPlainObject from './isPlainObject.js'
```
《[lodash源码分析之isObjectLike](isObjectLike.md)》
《[lodash源码分析之isPlainObject](isPlainObject.md)》

## 源码分析

`isElement` 用来判断 `value` 是否为一个 `Dom element` 对象。

```javascript
function isElement(value) {
  return isObjectLike(value) && value.nodeType === 1 && !isPlainObject(value)
}
```

首先判断 `value` 是否为类对象；再判断属性 `nodeType` 是否为 `1` ，`Dom Element `对象必定会有一个 `nodeType ` 为 `1` 的属性；接着再排除纯对象，因为 `Dom Element` 必定会继承某类元素对象。例如 `div` 会继承 `HTMLDivElement` 。

可以看出，这段代码是不严谨的，例如：

```javascript
function FakeElement () {
  this.nodeType = 1
}
const fakeElement = new FakeElement()
isElement(fakeElement) // => true
```

这个 [issue](https://github.com/lodash/lodash/issues/427) 解释了这样做的原因。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 