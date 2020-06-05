# lodash源码分析之propertyOf

本文为读 lodash 源码的第三百六十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseGet from './.internal/baseGet.js'
```

[《lodash源码分析之baseGet》](./internal/baseGet.md)


## 源码分析

`propertyOf` 和 `property` 类似，不过 `propertyOf` 接收的是 `object` 参数，它所创建的函数接收的是属性路径参数，调用时，最终得到的结果也是从 `object` 上将对应属性路径上的值取出。

源码如下：

```javascript
function propertyOf(object) {
  return (path) => object == null ? undefined : baseGet(object, path)
}
```

当传入的 `object` 为 `null` 或者 `undefined` 时，属性路径上肯定没有值，直接得到 `undefined` 。

否则调用 `baeseGet` 将 `object` 上对应属性路径上的值取出。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

