# lodash源码分析之cloneArrayBuffer

本文为读 lodash 源码的第一百八十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`cloneArrayBuffer` 的作用是用来复制 `arrayBuffer` 数据。

源码如下：

```javascript
function cloneArrayBuffer(arrayBuffer) {
  const result = new arrayBuffer.constructor(arrayBuffer.byteLength)
  new Uint8Array(result).set(new Uint8Array(arrayBuffer))
  return result
}
```

首先调用 `new arrayBuffer.constructor` ，传入 `arrayBuffer.byteLength` ，来创建一个新的 `arrayBuffer` 。使用 `arrayBuffer.constructor` 的原因是避免全局的构造函数遭到篡改。

因为不能直接操作 `arrayBuffer` 的内存，必须要在其上建立视图才可以操作。这里使用 `new Unit8Array` 的方式在 `result` ，也即新开辟的页面上建立一个视图，同样也在传入的 `arrayBuffer` 上建立一个视图。

再调用 `result` 视图上的 `set` 方法，将 `arrayBuffer` 写入 `result` 的内存，这样就达到了复制的目的。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 