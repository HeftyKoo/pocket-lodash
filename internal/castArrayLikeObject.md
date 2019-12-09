# lodash源码分析之castArrayLikeObject

本文为读 lodash 源码的第四十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import isArrayLikeObject from '../isArrayLikeObject.js'
```

[《lodash源码分析之isArrayLikeObject》](../isArrayLikeObject.md)

## 源码分析

这个函数的作用很简单，用来判断传入的值是不是一个 `arrayLikeObject` 类型，如果是，则返回传入的值，如果不是，则返回空数组。

源码如下：

```javascript
function castArrayLikeObject(value) {
  return isArrayLikeObject(value) ? value : []
}
```

`lodash` 主要使用这个工具函数，来避免一些参数传入不合规的情况下报错。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 