# lodash源码分析之cloneTypedArray

本文为读 lodash 源码的第一百八十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import cloneArrayBuffer from './cloneArrayBuffer.js'
```

[《lodash源码分析之cloneArrayBuffer》](./cloneArrayBuffer.md)

## 源码分析

`cloneTypedArray` 的作用是用来复制 `typedArray` 视图。

源码如下：

```javascript
function cloneTypedArray(typedArray, isDeep) {
  const buffer = isDeep ? cloneArrayBuffer(typedArray.buffer) : typedArray.buffer
  return new typedArray.constructor(buffer, typedArray.byteOffset, typedArray.length)
}
```

其实这个代码和 `cloneDataView` 一模一样，这也可以理解，因为 `typedArray` 和 `dataView` 一样，都是 `arrayBuffer` 的视图。

其实不需要两个独立的函数来做这个事情的，这两个完成可以合并成一个类似于 `cloneArrayBufferView` 之类的函数。

## 参考

[《lodash源码分析之cloneArrayBuffer》](./cloneDataView.md)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 