# lodash源码分析之pickBy

本文为读 lodash 源码的第三百篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import map from './map.js'
import basePickBy from './.internal/basePickBy.js'
import getAllKeysIn from './.internal/getAllKeysIn.js'
```

[lodash源码分析之map](./map.md)

[lodash源码分析之basePickBy](./internal/basePickBy.md)

[lodash源码分析之getAllKeysIn](./internal/getAllKeysIn.md)

## 源码分析

`pickBy` 和 `pick` 有点不太一样，`pick` 是通过传入属性来得到新的对象，`pickBy` 则是通过传入断言函数。

源码如下：

```javascript
function pickBy(object, predicate) {
  if (object == null) {
    return {}
  }
  const props = map(getAllKeysIn(object), (prop) => [prop])
  return basePickBy(object, props, (value, path) => predicate(value, path[0]))
}
```

和 `pick` 一样，如果 `object` 传入 `null` 或者不传，返回一个空对象。

使用 `getAllKeysIn` 获取对象及原型链上所有的属性（包括Symbol类型），并使用 `map` 将每个属性转换成数组，以符合 `basePickBy` 的要求。

最后调用 `basePickBy` ，但是这里的断言函数是使用用户传入的断言函数， `pickBy` 只负责将 `path` 从数组中取出来。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 