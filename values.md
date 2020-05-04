# lodash源码分析之values

本文为读 lodash 源码的第二百四十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseValues from './.internal/baseValues.js'
import keys from './keys.js'
```

[《lodash源码分析之baseValues》](internal/baseValues.md)

[《lodash源码分析之keys》](keys.md)

## 源码分析

`values` 的作用是将 `object` 上所有自身可枚举属性（`Symbol` 属性除外）的值取出。

源码如下：

```javascript
function values(object) {
  return object == null ? [] : baseValues(object, keys(object))
}
```

如果 `object` 没有传入，或者传入 `null` ，则返回空数组。

否则使用 `keys` 方法取出属性值，然后调用 `baseValues` 将所有值取出。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 