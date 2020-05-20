# lodash源码分析之baseUpdate

本文为读 lodash 源码的第三百零六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseGet from './baseGet.js'
import baseSet from './baseSet.js'
```

[lodash源码分析之baseGet](./baseGet.md)

[lodash源码分析之baseSet](./baseSet.md)


## 源码分析

`baseUpdate` 可以根据 `updater` 返回的值来设置路径属性 `path` 上的值，可以传入 `customizer` 来生成每一层的新对象，如果 `customizer` 返回 `undefined` ，则由内置逻辑处理。

源码如下：

```javascript
function baseUpdate(object, path, updater, customizer) {
  return baseSet(object, path, updater(baseGet(object, path)), customizer)
}
```

调用 `baseGet` 将路径的当前值取出，传给 `updater` 得到新值，然后使用 `baseSet` 来设置值，即可更新路径上的值。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 