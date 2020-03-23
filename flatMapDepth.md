# lodash源码分析之flatMapDepth

本文为读 lodash 源码的第一百四十七篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFlatten from './.internal/baseFlatten.js'
import map from './map.js'
```
[lodash源码分析之baseFlatten](internal/baseFlatten.md)
[lodash源码分析之map的实现](./map.md)

## 源码分析

`flatMapDepth` 其实是 `flattenDepth` + `map` ，先用 `map` 得到一个新的集合，然后再压平，可以指定压平的层级。

源码如下：

```javascript
function flatMapDepth(collection, iteratee, depth) {
  depth = depth === undefined ? 1 : +depth
  return baseFlatten(map(collection, iteratee), depth)
}
```

如果没有传 `depth`，则默认只压平一层。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 