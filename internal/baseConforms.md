# lodash源码分析之baseConforms

本文为读 lodash 源码的第三百四十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseConformsTo from './baseConformsTo.js'
import keys from '../keys.js'
```

[《lodash源码分析之baseConformsTo》](./baseConformsTo.md)

[《lodash源码分析之keys》](../keys.md)

## 源码分析

`baseConforms` 其实是 `baseConformsTo` 的偏函数，返回的函数作用和 `baseConformsTo` 一样，但是只接收 `obeject` 一个参数。

源码如下：

```javascript
function baseConforms(source) {
  const props = keys(source)
  return (object) => baseConformsTo(object, source, props)
}
```

`baseConforms` 接收一个参数 `source` ，并且用 `keys` 函数将 `source` 中的所有可枚举属性名取出。

然后返回一个函数，这个函数会调用 `baseConformsTo` ，接收 `object` 参数，因此返回的函数其实是和 `baseConformsTo` 等价的。

`baseConformsTo` 的分析如下：[《lodash源码分析之baseConformsTo》](./baseConformsTo.md)

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

