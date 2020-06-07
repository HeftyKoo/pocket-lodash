# lodash源码分析之toPath

本文为读 lodash 源码的第三百七十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import map from './map.js'
import copyArray from './.internal/copyArray.js'
import isSymbol from './isSymbol.js'
import stringToPath from './.internal/stringToPath.js'
import toKey from './.internal/toKey.js'
```

[《lodash源码分析之map》](./map.md)

[《lodash源码分析之copyArray》](./internal/copyArray.md)

[《lodash源码分析之isSymbol》](./isSymbol.md)

[《lodash源码分析之stringToPath》](./internal/stringToPath.md)

[《lodash源码分析之toKey》](./internal/toKey.md)

## 源码分析

`toPath` 的作用是将属性路径字符串转换成属性路径数组。

源码如下：

```javascript
function toPath(value) {
  if (Array.isArray(value)) {
    return map(value, toKey)
  }
  return isSymbol(value) ? [value] : copyArray(stringToPath(value))
}
```

如果 `value` 本身就是数组，则调用使用 `map` 将 `value` 遍历，调用 `toKey` 保证每一项都是合法的属性。

如果是 `Symbol` 类型，则使用数组包裹即可。

否则调用 `stringToPath` 将 `value` 转换成属性数组，因为 `stringToPath` 会缓存相同字符串转换出来的结果值，因此使用 `copyArray` 来切断引用。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

