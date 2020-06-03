# lodash源码分析之methodOf

本文为读 lodash 源码的第三百五十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import invoke from './invoke.js'
```

[《lodash源码分析之invoke》](./invoke.md)


## 源码分析

`methodOf` 和 `method` 的作用类似，不过接收的是 `object` 和 `args` ，返回的函数接收的参数是 `path` 。

源码如下：

```javascript
function methodOf(object, args) {
  return (path) => invoke(object, path, args)
}
```

其实返回的函数也是调用 `invoke` 。

`method` 分析见：[《lodash源码分析之method》](./method.md)

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

