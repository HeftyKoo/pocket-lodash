# lodash源码分析之basePickBy

本文为读 lodash 源码的第二百九十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseGet from './baseGet.js'
import baseSet from './baseSet.js'
import castPath from './castPath.js'
```

[lodash源码分析之baseGet](./baseGet.md)

[lodash源码分析之baseSet](./baseSet.md)

[lodash源码分析之castPath](./castPath.md)


## 源码分析

`basePickBy` 是实现 `pick` 和 `pickBy` 的内部方法，可以传入一组路径，然后从源 `object` 上取出对应的值，得到一个新的对象。

取出的路径和值需要通过断言函数 `predicate` 的判断。

源码如下：

```javascript
function basePickBy(object, paths, predicate) {
  let index = -1
  const length = paths.length
  const result = {}

  while (++index < length) {
    const path = paths[index]
    const value = baseGet(object, path)
    if (predicate(value, path)) {
      baseSet(result, castPath(path, object), value)
    }
  }
  return result
}
```

先是初始化一个对象 `result` 作为结果对象。

遍历 `paths`，调用 `baseGet` 方法从源对象上取出对应路径的值，将值和路径传给断言函数 `predicate` ，如果断言函数返回的是真值，则在结果对象对应的路径上设置值 `value` 。

遍历完成后，即取出了所有路径上符号条件的值，设置到了新的对象上。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 