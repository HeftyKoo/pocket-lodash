# lodash源码分析之copySymbolsIn

本文为读 lodash 源码的第一百九十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import copyObject from './copyObject.js'
import getSymbolsIn from './getSymbolsIn.js'
```

[《lodash源码分析之copyObject》](./copyObject.md)
[《lodash源码分析之getSymbolsIn》](./getSymbolsIn.md)

## 源码分析

`copySymbolsIn` 和 `copySymbols` 的作用类型，不过会将原型链上的 `Symbol` 类型的属性也复制到目标 `object` 上。

源码如下：

```javascript
function copySymbolsIn(source, object) {
  return copyObject(source, getSymbolsIn(source), object)
}
```

可以看到，其实源码跟 `copySymbols` 差不多，只不过这里使用 `getSymbolsIn` 来收集 `Symbol` 类型的属性，会将原型链上的 `Symbol` 属性都收集回来。

## 相关资料

[《lodash源码分析之copySymbols》](./copySymbols.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 