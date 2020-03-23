# lodash源码分析之flatMapDeep

本文为读 lodash 源码的第一百四十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseFlatten from './.internal/baseFlatten.js'
import map from './map.js'
```
[lodash源码分析之baseFlatten](internal/baseFlatten.md)
[lodash源码分析之map的实现](./map.md)

## 源码分析

`flatMapDeep` 其实是 `flattenDeep` + `map` ，先用 `map` 得到一个新的集合，然后再压平，可以压平成只有一层。

源码如下：

```javascript
const INFINITY = 1 / 0
function flatMapDeep(collection, iteratee) {
  return baseFlatten(map(collection, iteratee), INFINITY)
}
```

可以看到 `baseFlatten` 传入的层级是 `INFINITY` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 