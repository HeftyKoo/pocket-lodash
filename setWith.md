# lodash源码分析之setWith

本文为读 lodash 源码的第三百零三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseSet from './.internal/baseSet.js'
```

[lodash源码分析之baseSet](./internal/baseSet.md)

## 源码分析

`setWith` 和 `set` 差不多，但是可以传入自定义设置函数 `csutomizer` 来设置值。

源码如下：

```javascript
function setWith(object, path, value, customizer) {
  customizer = typeof customizer === 'function' ? customizer : undefined
  return object == null ? object : baseSet(object, path, value, customizer)
}
```

先判断 `customizer` 是不是函数，如果不是，将 `customizer` 重置为 `undefined` ，即使用内置的设置逻辑。

如果 `object` 不为 `null` 或者 `undefined` ，则调用 `baseSet` 来设置值。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 