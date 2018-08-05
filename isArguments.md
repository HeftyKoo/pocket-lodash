# lodash源码分析之isArguments

> 有人命中注定要过平庸的生活，默默无闻，因为他们经历了痛苦或不幸；有人却故意这样做，那是因为他们得到的幸福超过了他们的承受能力。
>
> ——卡尔维诺《烟云》

本文为读 lodash 源码的第二十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import getTag from './.internal/getTag.js'
import isObjectLike from './isObjectLike'
```

《[lodash源码分析之数据类型获取的兼容性](./internal/getTag.md)》

《[lodash源码分析之isObjectLike](isObjectLike.md)》

## 源码分析

```javascript
function isArguments(value) {
  return isObjectLike(value) && getTag(value) == '[object Arguments]'
}
```

`isArguments` 用来判断某个值是否为类 `arguments` 对象。

如果某个值为类对象（使用 `isObjectLike` 判断），并且调用 `Object.prototype.toString` 返回的值为 `[object Arguments]` 时，则为类 `arguments` 对象。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面