# lodash源码分析之copySymbols

本文为读 lodash 源码的第一百九十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import copyObject from './copyObject.js'
import getSymbols from './getSymbols.js'
```

[《lodash源码分析之copyObject》](./copyObject.md)
[《lodash源码分析之getSymbols》](./getSymbols.md)

## 源码分析

`copySymbols` 的作用是将对象 `source` 上的 `Symbol` 类型的属性复制到对象 `object` 上。

源码如下：

```javascript
function copySymbols(source, object) {
  return copyObject(source, getSymbols(source), object)
}
```

其实就是调用 `getSymbols` 将 `source` 上可枚举的 `Symbol` 属性提取出来，然后调用 `copyObject` 将所有的 `Symbols` 属性的值进行复制。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 