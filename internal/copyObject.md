# lodash源码分析之copyObject

本文为读 lodash 源码的第一百八十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import assignValue from './assignValue.js'
import baseAssignValue from './baseAssignValue.js'
```

[《lodash源码分析之assignValue》](./assignValue.md)
[《lodash源码分析之baseAssignValue》](./baseAssignValue.md)

## 源码分析

`copyObject` 用来将对象 `source` 上所有的 `props` 属性复制到 `object` 上，支持传入一个函数 `customizer` 来计算得到新的值。

源码如下：

```javascript
function copyObject(source, props, object, customizer) {
  const isNew = !object
  object || (object = {})

  for (const key of props) {
    let newValue = customizer
      ? customizer(object[key], source[key], key, object, source)
      : undefined

    if (newValue === undefined) {
      newValue = source[key]
    }
    if (isNew) {
      baseAssignValue(object, key, newValue)
    } else {
      assignValue(object, key, newValue)
    }
  }
  return object
}
```

这里用一个标志 `isNew` 来标记有没有传入 `object` ，如果没有传入 `object` ，会将 `object` 初始化为空对象。

`isNew` 这个标记用来做性能优化，下面会讲到。

接着就是遍历 `props`，如果有传入 `customizer` 函数，则调用 `customizer` 函数，将当前 `key` 的目标当前值 `object[key]` 和源值 `source[key]` 、`key`、`object`、`source` 都传给 `customizer` 函数，返回的结果赋值给 `newValue` 变量。如果没有传入 `customizer` 函数，则 `newValue` 为 `undefined`。

为什么不在这里直接从 `source` 中直接将对应的 `key` 值取出呢？因为 `customizer` 函数也可能会返回 `undefined` 值。

因此后面直接判断 `newValue === undefined` 的时候，再从 `souce` 中取出对应 `key` 的值，赋值给 `newValue` 。

接下来，就可以看到 `isNew` 的作用了。如果 `isNew` 为 `true` ，则调用 `baseAssignValue` 来更新 `object` 上对应属性 `key` 的值，否则使用 `assignValue` 。

这两个函数有什么区别呢？ `baseAssignValue` 不会做属性是否为自身属性的检测，因此相对 `assignValue` 来说性能更好。

因为一个全新的对象自身是不会有属性的，因为完全不需要这个检测，可以直接使用 `baseAssignValue` ，这样性能更好。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 