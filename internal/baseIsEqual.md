# lodash源码分析之baseIsEqual

本文为读 lodash 源码的第二百一十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIsEqualDeep from './baseIsEqualDeep.js'
import isObjectLike from '../isObjectLike.js'
```

[《lodash源码分析之baseIsEqualDeep》](./baseIsEqualDeep.md)
[《lodash源码分析之isObjectLike》](./isObjectLike.md)

## 源码分析

经过之前一系列内部函数的铺垫，`baseIsEqual` 的实现就相对简单了。

源码如下：

```javascript
function baseIsEqual(value, other, bitmask, customizer, stack) {
  if (value === other) {
    return true
  }
  if (value == null || other == null || (!isObjectLike(value) && !isObjectLike(other))) {
    return value !== value && other !== other
  }
  return baseIsEqualDeep(value, other, bitmask, customizer, baseIsEqual, stack)
}
```

如果 `value` 和 `other` 经过是全等的，这是最简单的了，两个值肯定相等。

接下来再比较 `NaN` ，如果两个值 `value !== value` 和 `other !== other` 都为 `true` ，表示这两个值都为 `NaN` ，也返回 `true` 。

进行了简单的比较后，依然得不到结果，则调用 `baseIsEqualDeep` 来比较，这里要注意第五个参数，也即之前经常出来的 `equalFunc` ，传入的是 `baseIsEqual` ，这也是之前一系列函数会递归的原因。


## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 