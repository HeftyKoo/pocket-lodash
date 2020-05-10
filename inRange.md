# lodash源码分析之inRange

本文为读 lodash 源码的第二百七十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseInRange from './.internal/baseInRange.js'
```

[《lodash源码分析之baseInRange》](internal/baseInRange.md)

## 源码分析

`inRange` 用来检测 `number` 是否在 `start` 和 `end` 的范围内。

源码如下：

```javascript
function inRange(number, start, end) {
  if (end === undefined) {
    end = start
    start = 0
  }
  return baseInRange(+number, +start, +end)
}
```

其实就是调用 `baseInRange` 来实现，因为 `baseInRange` 不会处理参数，因此 `inRange` 负责做参数处理的工作。

如果没有传入 `end` ，则将 `start` 的值赋值给 `end` ，将 `start` 设置为 `0` 。

在将参数传给 `baseInRange` 函数时，会将 `number` 、`start` 和 `end` 转换为数字。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

