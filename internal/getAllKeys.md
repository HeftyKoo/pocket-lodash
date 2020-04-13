# lodash源码分析之getAllKeys

本文为读 lodash 源码的第一百九十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getSymbols from './getSymbols.js'
import keys from '../keys.js'
```

[《lodash源码分析之getSymbols》](./getSymbols.md)
[《lodash源码分析之keys》](../keys.md)

## 源码分析

`getAllKeys` 的作用跟 `keys` 的作用差不多，但是 `keys` 不会获取对象 `object` 上的 `Symbol` 类型的属性，`getAllKeys` 会将 `Symbol` 类型的属性都返回 。

源码如下：

```javascript
function getAllKeys(object) {
  const result = keys(object)
  if (!Array.isArray(object)) {
    result.push(...getSymbols(object))
  }
  return result
}
```

首先调用 `keys` 得到所有非 `Symbol` 类型的属性，然后判断如果为非数组类型，则调用 `getSymbols` 获取所有的 `Symbol` 类型的属性。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 