# lodash源码分析之pullAll

本文为读 lodash 源码的第六十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import basePullAll from './.internal/basePullAll.js'
```

[《lodash源码分析之basePullAll》](internal/basePullAll.md)

## 源码分析

```javascript
function pullAll(array, values) {
  return (array != null && array.length && values != null && values.length)
    ? basePullAll(array, values)
    : array
}
```

有了 `basePullAll` ，要实现 `pullAll` 就简单了。

其实 `pullAll` 最终调用的是 `basePullAll` ，只不过在调用之前，对 `array` 和 `values` 做了一些基本的检测。

```javascript
array != null && array.length
```

确保 `array` 有传，并且不为空

```javascript
values != null && values.length
```

确保 `values` 有传，并且不为空

如果条件不满足，则直接返回 `array` ，否则调用 `basePullAll` 。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 