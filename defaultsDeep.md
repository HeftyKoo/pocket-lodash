# lodash源码分析之defaultsDeep

本文为读 lodash 源码的第二百八十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import customDefaultsMerge from './.internal/customDefaultsMerge.js'
import mergeWith from './mergeWith.js'
```

[lodash源码分析之customDefaultsMerge](./internal/customDefaultsMerge.md)

[lodash源码分析之mergeWith](./mergeWith.md)


## 源码分析

`defaultsDeep` 的作用和 `default` 类似，不过会深层设置默认值。

源码如下：

```javascript
function defaultsDeep(...args) {
  args.push(undefined, customDefaultsMerge)
  return mergeWith.apply(undefined, args)
}
```

其实就个过程类似于 `merge` ，但是又不能简单地 `merge` ，只在相关属性的值为 `undefined` 或者没有相关属性时才会合并值。

因此 `defaultsDeep` 调用了 `mergeWith` ，但是使用了自定义合并函数 `customDefaultsMerge` 。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 