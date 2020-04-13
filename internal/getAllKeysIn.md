# lodash源码分析之getAllKeysIn

本文为读 lodash 源码的第一百九十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getSymbolsIn from './getSymbolsIn.js'
```

[《lodash源码分析之getSymbolsIn》](./getSymbolsIn.md)

## 源码分析

`getAllKeysIn` 和 `getAllKeys` 类似，都是收集 `object`  上包括 `Symbol` 类型的属性。

但是 `getAllKeysIn` 除了收集自身的属性外，还会收集原型链上的属性。

源码如下：

```javascript
function getAllKeysIn(object) {
  const result = []
  for (const key in object) {
    result.push(key)
  }
  if (!Array.isArray(object)) {
    result.push(...getSymbolsIn(object))
  }
  return result
}
```

可以看到，首先用 `for...in` 来收集非 `Symbol` 类型的属性，因为 `for...in` 会遍历原型链。

接着，非数组类型的 `object` 会调用 `getSymbolsIn` 来收集 `Symbol` 类型的属性。

## 相关资料

[《lodash源码分析之getAllKeys》](./getAllKeys.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 