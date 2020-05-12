# lodash源码分析之at

本文为读 lodash 源码的第二百七十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseAt from './.internal/baseAt.js'
import baseFlatten from './.internal/baseFlatten.js'
```

[《lodash源码分析之baseAt》](internal/baseAt.md)
[《lodash源码分析之baseFlatten》](internal/baseFlatten.md)

## 源码分析

`at` 可以根据传入的一组路径，取出路径对应的一组值。

源码如下：

```javascript
const at = (object, ...paths) => baseAt(object, baseFlatten(paths, 1))
```

首先用 `...` 收集到 `paths` 到一个数组中，因为 `at` 支持不定参数，可以传入路径数组，也可以将一个一个路径作为单独的参数传入。

所以在调用 `baseAt` 之前，先调用 `baseFlatten` 将 `paths` 摊平一层。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 