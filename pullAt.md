# lodash源码分析之pullAt

本文为读 lodash 源码的第七十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import map from './map.js'
import baseAt from './.internal/baseAt.js'
import basePullAt from './.internal/basePullAt.js'
import compareAscending from './.internal/compareAscending.js'
import isIndex from './.internal/isIndex.js'
```

[《lodash源码分析之map的实现》](map.md)

[《lodash源码分析之baseAt》](internal/baseAt.md)

[《lodash源码分析之basePullAt》](internal/basePullAt.md)

[《lodash源码分析之compareAscending》](internal/compareAscending.md)

[《lodash源码分析之isIndex》](internal/isIndex.md)

## 源码分析

`pullAt` 会将数组 `array` 根据指定的索引 `indexes` 的值删除，并且返回所有删除的值。

具体源码如下：

```javascript
function pullAt(array, ...indexes) {
  const length = array == null ? 0 : array.length
  const result = baseAt(array, indexes)

  basePullAt(array, map(indexes, (index) => isIndex(index, length) ? +index : index).sort(compareAscending))
  return result
}
```

`pullAt` 其实依赖了两个基础的内部函数 `baseAt` 和 `basePullAt`。

在分析 `basePullAt` 的时候，我们已经知道，`basePullAt` 只负责从原数组中删除指定索引值的工作，返回的是原数组，并没有返回所删除的值。

但是 `pullAt` 要求的是返回所删除的值，那这部分工作由谁来完成呢？

其实这部分工作由 `baseAt` 来完成，在将指定的索引删除之前，就从原数组中使用 `baseAt` 将对应索引的值取出，保存在 `result` 中，这是最终要返回的结果，然后再调用 `basePullAt` 去更改原数组。

从 `basePullAt` 的分析中可以知道，`indexes` 可以不是索引，也可以是属性，因此使用 `map` 函数，将每一项都使用 `isIndex` 来判断是不是索引，如果是索引，则使用 `+index` 转换一下，确保索引为 `number` 类型。

又由于 `basePullAt` 需要确保 `indexes` 为升序排列，因此使用 `sort` 和比较器 `compareAscending` 来进行排序。  

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 