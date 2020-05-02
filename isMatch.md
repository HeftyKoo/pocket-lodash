# lodash源码分析之isMatch

本文为读 lodash 源码的第二百二十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIsMatch from './.internal/baseIsMatch.js'
import getMatchData from './.internal/getMatchData.js'
```

[《lodash源码分析之baseIsMatch》](internal/baseIsMatch.md)
[《lodash源码分析之getMatchData》](internal/getMatchData.md)

## 源码分析

`isMath` 用来判断 `object` 是否包含 `source` ，基本上是调用 `baseIsMatch` 来实现，源码如下：

```javascript
function isMatch(object, source) {
  return object === source || baseIsMatch(object, source, getMatchData(source))
}
```

如果 `object === source` ，那 `object` 肯定是包含 `source` 的，否则调用 `baseIsMath` 来得到结果。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 