# lodash源码分析之isString

本文为读 lodash 源码的第一百六十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
```

[《lodash源码分析之getTag》](internal/getTag.md)

## 源码分析

`isString` 用来判断传入的 `value` 值是否为 `string` 类型。

源码如下：

```javascript
function isString(value) {
  const type = typeof value
  return type === 'string' || (type === 'object' && value != null && !Array.isArray(value) && getTag(value) == '[object String]')
}
```

如果对于一判的字面量判断，直接用 `typeof value === 'string'` 即可以判断是否为 `string` 类型。

但是对于使用 `new String()` 构造函数创建出来的字符串对象，使用 `typeof` 检测时，得到的结果会是 `object` 。

因此对于这种情况，需要使用 `Object.prototype.toString.call` 来检测，也即 `lodash` 封装好的函数 `getTag`。

同时，出于性能的考虑，在调用 `getTag` 检测之前，排除掉 `value` 为 `null` 和 `undefined` 的情况，也即 `value != null` ，还有排除数组的情况，也即 `!Array.isArray(value)` ，避免调用 `getTag` 做太多无效的检测，影响性能。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 