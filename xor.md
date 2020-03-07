# lodash源码分析之xor

本文为读 lodash 源码的第一百一十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseXor from './.internal/baseXor.js'
import isArrayLikeObject from './isArrayLikeObject.js'
```
[《lodash源码分析之baseXor》](internal/baseXor.md)
[《lodash源码分析之isArrayLikeObject》](isArrayLikeObject.md)

## 源码分析

`xor` 调用的是 `baseXor` ，但是传入的不是数组集，而是不定参数，每个参数都应该为数组或类数组，如果不是数组会被过滤掉。

源码如下：

```javascript
function xor(...arrays) {
  return baseXor(arrays.filter(isArrayLikeObject))
}
```

首先使用 `...` 操作符将参数收集在 `arrays` 中。

然后调用数组的 `filter` 方法，将非数组或类数组的项过滤掉，再调用 `baseXor` 得出结果。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 