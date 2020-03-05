# lodash源码分析之unzipWith

本文为读 lodash 源码的第一百零八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import map from './map.js'
import unzip from './unzip.js'
```
[《lodash源码分析之map的实现》](map.md)
[《lodash源码分析之unzip》](unzip.md)

## 源码分析

`unzipWith` 的作用和 `zip` 差不多，但是接收多一个 `iteratee` 参数，可以能每一个分组进行转换，`iteratee` 接入的参数为每一个分组。

源码如下：

```javascript
function unzipWith(array, iteratee) {
  if (!(array != null && array.length)) {
    return []
  }
  const result = unzip(array)
  return map(result, (group) => iteratee.apply(undefined, group))
}
```

可以看到，`unzipWith` 首先调用 `unzip` 得出结果。

然后再调用 `map` 取出每一个分组，然后将每一个分组传给 `iterate` 处理。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 