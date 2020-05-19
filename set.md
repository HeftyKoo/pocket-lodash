# lodash源码分析之set

本文为读 lodash 源码的第三百零二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSet from './.internal/baseSet.js'
```

[lodash源码分析之baseSet](./internal/baseSet.md)

## 源码分析

`set` 可以将值设置到 `object` 指定的属性路径上。

源码如下：

```javascript
function set(object, path, value) {
  return object == null ? object : baseSet(object, path, value)
}
```

如果 `object` 为 `null` 或者 `undefined` ，直接将原值返回。

否则直接调用 `baseSet` 来设置值。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 