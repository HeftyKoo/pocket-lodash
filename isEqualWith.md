# lodash源码分析之isEqualWith

本文为读 lodash 源码的第二百二十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIsEqual from './.internal/baseIsEqual.js'
```

[《lodash源码分析之baseIsEqual》](internal/baseIsEqual.md)

## 源码分析

```javascript
function isEqualWith(value, other, customizer) {
  customizer = typeof customizer === 'function' ? customizer : undefined
  const result = customizer ? customizer(value, other) : undefined
  return result === undefined ? baseIsEqual(value, other, undefined, customizer) : !!result
}
```

`isEqualWith` 比 `eqDeep` 传多一个 `customizer` 函数，如果这个参数没有传，或者传入的不是函数，则和 `eqDeep` 的效果完全相同。

如果有传入 `customizer` 函数，则会先调用 `customizer` 函数，如果 `customizer` 函数返回的结果不是 `undefined` ，则直接返回 `!!result` 。

如果是 `undefined` ，则调用 `baseIsEqual` 来进行比较，并且将 `customizer` 函数传入 `baseIsEqual` 。


## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 