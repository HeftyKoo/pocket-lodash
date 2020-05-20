# lodash源码分析之update

本文为读 lodash 源码的第三百零七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseUpdate from './.internal/baseUpdate.js'
```

[lodash源码分析之baseUpdate](./internal/baseUpdate.md)


## 源码分析

`update` 用来更新路径 `path` 下的值。

源码如下：

```javascript
function update(object, path, updater) {
  return object == null ? object : baseUpdate(object, path, updater)
}
```

其实就是调用 `baseUpdate` 来实现，但是会在上层对 `object` 做一层校验。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 