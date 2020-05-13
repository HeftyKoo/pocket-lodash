# lodash源码分析之assignMergeValue

本文为读 lodash 源码的第二百七十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseAssignValue from './baseAssignValue.js'
import eq from '../eq.js'
```

[lodash源码分析之baseAssignValue](./baseAssignValue.md)

[lodash源码分析之eq](../eq.md)

## 源码分析

`assignMergeValue` 的作用是将 `object` 上属性 `key` 的值更新为 `value` 。

源码如下：

```javascript
function assignMergeValue(object, key, value) {
  if ((value !== undefined && !eq(object[key], value)) ||
      (value === undefined && !(key in object))) {
    baseAssignValue(object, key, value)
  }
}
```

看到更新有两个条件：

一是 `value` 不为 `undefined` ，并且 `object` 上原来的属性 `key` 的值和传入的值不相等时，就需要更新。

或者 `value` 为 `undefined` 时，属性 `key` 在 `object` 或者 `object` 的原型链上并不存在时。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 