# lodash源码分析之isMatchWith

本文为读 lodash 源码的第二百二十九篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIsMatch from './.internal/baseIsMatch.js'
import getMatchData from './.internal/getMatchData.js'
```

[《lodash源码分析之baseIsMatch》](internal/baseIsMatch.md)
[《lodash源码分析之getMatchData》](internal/getMatchData.md)

## 源码分析

`isMathWith` 的作用和 `isMatch` 一样，只不过 `isMatchWith` 支持自定义比较函数，如果不传自定义比较函数，效果和 `isMath` 一样。

源码如下：

```javascript
function isMatchWith(object, source, customizer) {
  customizer = typeof customizer === 'function' ? customizer : undefined
  return baseIsMatch(object, source, getMatchData(source), customizer)
}
```

如果传入的 `customizer` 不是函数，则将 `customizer` 重置为 `undefined`，避免调用 `baseIsMatch` 时出错。

最后调用 `baseIsMatch` 得到结果。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 