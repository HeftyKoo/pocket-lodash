# lodash源码分析之rangeRight

本文为读 lodash 源码的第三百六十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import createRange from './.internal/createRange.js'
```

[lodash源码分析之createRange](./createRange.md)

## 源码分析

`rangeRight` 和 `range` 作用类似，不过排序方式相反 。

源码如下：

```javascript
const rangeRight = createRange(true)
```

其实就是调用 `createRange` 来实现，`fromRight` 传入的为 `true`。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

