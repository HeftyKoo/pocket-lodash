# lodash源码分析之customDefaultsMerge

本文为读 lodash 源码的第二百八十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseMerge from './baseMerge.js'
import isObject from '../isObject.js'
```

[lodash源码分析之baseMerge](./baseMerge.md)

[lodash源码分析之isObject](../isObject.md)


## 源码分析

`customDefaultsMerge` 会被 `defaultsDeep` 使用， `defaultsDeep ` 可以合并多个对象， `customDefaultsDeep` 只合并两个对象。

源码如下：

```javascript
function customDefaultsMerge(objValue, srcValue, key, object, source, stack) {
  if (isObject(objValue) && isObject(srcValue)) {
    stack.set(srcValue, objValue)
    baseMerge(objValue, srcValue, undefined, customDefaultsMerge, stack)
    stack['delete'](srcValue)
  }
  return objValue
}
```

因为调用的时候会和其它函数统一接口，因此会有一些多余的参数。

其实就是调用 `baseMerge` 来深度合并 `objValue` 和 `srcValue` ，这里用了 `stack` 来防止循环依赖的问题。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 