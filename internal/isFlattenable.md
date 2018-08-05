# lodash源码分析之isFlattenable

> 这时我回头看看鲁迅先生：老先生的相貌先就长得不一样。这张脸非常不买账，又非常无所谓，非常酷，又非常慈悲，看上去一脸的清苦、刚直、坦然，骨子里却透着风流与俏皮……可是他拍照片似乎不做什么表情，就那么对着镜头，意思是说：怎么样？我就是这样。
>
> ——陈丹青《笑谈大先生》

本文为读 lodash 源码的第二十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isArguments from '../isArguments.js'
```

《[lodash源码分析之isArguments](../isArguments.md)》

## 源码分析

```javascript
const spreadableSymbol = Symbol.isConcatSpreadable

function isFlattenable(value) {
  return Array.isArray(value) || isArguments(value) ||
    !!(spreadableSymbol && value && value[spreadableSymbol])
}
```

lodash 用 `isFlattenable` 内部函数来判断某个值是否可以被展平。

明显数组和类数组的 `arguments` 对象，可以通过遍历来展平。

另外在 `ES6` 中，可以设置 `Symbol.isConcatSpreadable` 的属性来表示该对象是否可以被展平。 `Symbol.isConcatSpreadable` 的值如果被设置为真值时，该对象是可以被展平的。

例如：

```javascript
const a = {
    0:0,
    1:1,
    length: 2
}
console.log([].concat(a)) // [{0:0,1:1,length:2}]
a[Symbol.isConcatSpreadable] = true
console.log([].concat(a)) // [0,1]
```

当设置 `a` 的 `Symbol.isConcatSpreadable` 的属性值为 `true` 后，`a` 就可以被展平了。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面