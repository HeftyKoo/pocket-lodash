# lodash源码分析之conformsTo

本文为读 lodash 源码的第二百零四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseConformsTo from './.internal/baseConformsTo.js'
import keys from './keys.js'
```

[《lodash源码分析之baseConformsTo》](internal/baseConformsTo.md)
[《lodash源码分析之keys》](keys.md)

## 源码分析

`conformsTo` 会对 `object` 上特定的属性的值做检测，`conformsTo` 会调用 `source` 上的断言函数检测 `object` 上对应属性的值。

源码如下：

```javascript
function conformsTo(object, source) {
  return source == null || baseConformsTo(object, source, keys(source))
}
```

如果传入的 `source` 为 `null` 或者 `undefined`，表示没有对应的断言函数，可直接返回 `true` 。

调用 `keys` 收集 `source` 上的所有属性，调用 `baseConformsTo` 即可得出结果。主要逻辑由 `baseConformsTo` 完成。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 