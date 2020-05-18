# lodash源码分析之pick

本文为读 lodash 源码的第二百九十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import basePick from './.internal/basePick.js'
```

[lodash源码分析之basePick](./internal/basePick.md)

## 源码分析

`pick` 可以从 `object` 上将传入的属性上的值取出，得到一个新的对象返回。

源码如下：

```javascript
function pick(object, ...paths) {
  return object == null ? {} : basePick(object, paths)
}
```

使用 `...` 操作符将不定参数收集到 `paths` 中，得到一个数组。因为后面会调用 `basePick` ，`basePick` 接收的路径数组。

如果 `object` 没有传入，或者传入的是 `null` ，会返回一个空对象，否则调用 `basePick` 得到结果。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 