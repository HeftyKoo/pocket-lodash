# lodash源码分析之flowRight

本文为读 lodash 源码的第三百五十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import flow from './flow.js'
```

[《lodash源码分析之flow》](./flow.md)

## 源码分析

`flowRight` 和 `flow` 的作用类似，只不过 `funcs` 调用的顺序和传入的顺序相反。

源码如下：

```javascript
function flowRight(...funcs) {
  return flow(...funcs.reverse())
}
```

其实就是调用 `flow` ，只不过在将 `funcs` 传入 `flows` 时，先调用 `reverse` 方法，将顺序反转。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

