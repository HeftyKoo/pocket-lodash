# lodash源码分析之basePick

本文为读 lodash 源码的第二百九十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import basePickBy from './basePickBy.js'
import hasIn from '../hasIn.js'
```

[lodash源码分析之basePickBy](./basePickBy.md)

[lodash源码分析之hasIn](../hasIn.md)


## 源码分析

`basePick` 是实现 `pick` 的内部方法。

源码如下：

```javascript
function basePick(object, paths) {
  return basePickBy(object, paths, (value, path) => hasIn(object, path))
}
```

其实是调用 `basePickBy` 来实现，但是 `pick` 是不支持传入断言函数的，因此 `basePick` 会传给 `basePickBy` 的一个断言函数，这个断言函数的作用仅仅是检测属性 `path` 是否在 `object` 或者 `object` 的原型链上。

因为 `basePickBy` 会使用 `baseGet` 取值，即使 `object` 上没有该属性，也会在新对象对应的属性上设置一个 `undefined` 值，因此这里需要做个检测。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 