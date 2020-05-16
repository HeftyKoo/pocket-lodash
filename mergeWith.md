# lodash源码分析之mergeWith

本文为读 lodash 源码的第二百八十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseMerge from './.internal/baseMerge.js'
import createAssigner from './.internal/createAssigner.js'
```

[lodash源码分析之baseMerge](./internal/baseMerge.md)

[lodash源码分析之createAssigner](./internal/createAssigner.md)


## 源码分析

`mergeWith` 用来合并多个对象的属性到 `object` 上。

源码如下：

```javascript
const mergeWith = createAssigner((object, source, srcIndex, customizer) => {
  baseMerge(object, source, srcIndex, customizer)
})
```

这里 `createAssigner` 类似于迭代器，迭代每个 `source` ，在迭代过程中调用 `baseMerge` ，将每个 `source` 依次合并到 `object` 上。

这里 `baseMerge` 会传入 `customizer` 函数。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 