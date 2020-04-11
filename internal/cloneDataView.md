# lodash源码分析之cloneDataView

本文为读 lodash 源码的第一百八十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import cloneArrayBuffer from './cloneArrayBuffer.js'
```

[《lodash源码分析之cloneArrayBuffer》](./cloneArrayBuffer.md)

## 源码分析

`cloneDataView` 的作用是用来复制 `dataView` 视图。

源码如下：

```javascript
function cloneDataView(dataView, isDeep) {
  const buffer = isDeep ? cloneArrayBuffer(dataView.buffer) : dataView.buffer
  return new dataView.constructor(buffer, dataView.byteOffset, dataView.byteLength)
}
```

由于 `dataView` 只是 `arrayBuffer` 上的一种视图，如果不需要深度复制，则直接使用原来的内存 `dataView.buffer` ，在原来的内存上新建一个 `dataView` 视图即可。

如果需要深度复制，则调用 `cloneArrayBuffer` ，将原来的 `buffer` 复制到一块新的内存，再在这块内存上创建 `dataView` 视图。

`dataView` 视图可能操作的不是整块内存，因此在构建新的 `dataView` 视图时，要传入 `dataView.byteOffset` 来指定和原来视图一样的开始位置。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 