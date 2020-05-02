# lodash源码分析之isStrictComparable

本文为读 lodash 源码的第二百二十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isObject from '../isObject.js'
```

[《lodash源码分析之isObject》](../isObject.md)

## 源码分析

`isStrictComparable` 用来判断一个值是否可以使用 `===` 比较。

源码如下：

```javascript
function isStrictComparable(value) {
  return value === value && !isObject(value)
}
```

`value === value` 其实是排除了 `NaN`。

`!isObject` 排除了除了 `null` 以外的 `typeof value === 'object'` 类型和函数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 