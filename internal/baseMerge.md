# lodash源码分析之baseMerge

本文为读 lodash 源码的第二百七十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import Stack from './Stack.js'
import assignMergeValue from './assignMergeValue.js'
import baseFor from './baseFor.js'
import baseMergeDeep from './baseMergeDeep.js'
import isObject from '../isObject.js'
import keysIn from '../keysIn.js'
```

[lodash源码分析之Stack](./Stack.md)

[lodash源码分析之assignMergeValue](./assignMergeValue.md)

[lodash源码分析之baseFor](./baseFor.md)

[lodash源码分析之baseMergeDeep](./baseMergeDeep.md)

[lodash源码分析之isObject](../isObject.md)

[lodash源码分析之keysIn](../keysIn.md)


## 源码分析

`baseMerge` 的作用是将 `srouce` 上的所有属性都合并到 `object` 上。

源码如下：

```javascript
function baseMerge(object, source, srcIndex, customizer, stack) {
  if (object === source) {
    return
  }
  baseFor(source, (srcValue, key) => {
    if (isObject(srcValue)) {
      stack || (stack = new Stack)
      baseMergeDeep(object, source, key, srcIndex, baseMerge, customizer, stack)
    }
    else {
      let newValue = customizer
        ? customizer(object[key], srcValue, `${key}`, object, source, stack)
        : undefined

      if (newValue === undefined) {
        newValue = srcValue
      }
      assignMergeValue(object, key, newValue)
    }
  }, keysIn)
}
```

如果 `object === source` 表示两者相等，则不需要再合并，直接返回即可。

接着调用 `baseFor` 来遍历 `source` 上所有的属性，得到每个属性上的值 `srcValue` 和属性 `key` 。

接着判断 `srcValue` 是否为对象，如果是，则调用 `baseMergeDeep` 来合并每个属性上的值，注意这里的 `mergeFunc` 传入的是 `baseMerge` ，这也是产生递归深度复制的原因。

如果是其它值类型，则先判断有没有传入自定义合并函数 `customizer` ，如果有，则调用 `customizer` 得到合并后的值 `newValue` 。

如果 `newValue` 为 `undefined`，则表示没有传 `customizer` 函数或者 `customizer` 函数返回的值为 `undefined`，这种情况下，合并后的值直接就为 `srcValue` 。

最后调用 `assignMergeValue` 将 `newValue`  合并到 `object` 对应的 `key` 上即可。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 