# lodash源码分析之baseMatches

本文为读 lodash 源码的第三百五十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import baseIsMatch from './baseIsMatch.js'
import getMatchData from './getMatchData.js'
import matchesStrictComparable from './matchesStrictComparable.js'
```

[《lodash源码分析之baseIsMatch》](./baseIsMatch.md)

[《lodash源码分析之getMatchData》](./getMatchData.md)

[《lodash源码分析之matchesStrictComparable》](./matchesStrictComparable.md)

## 源码分析

`baseMatches` 相当于 `baseIsMatch` 的偏函数。

源码如下：

```javascript
function baseMatches(source) {
  const matchData = getMatchData(source)
  if (matchData.length === 1 && matchData[0][2]) {
    return matchesStrictComparable(matchData[0][0], matchData[0][1])
  }
  return (object) => object === source || baseIsMatch(object, source, matchData)
}
```

调用 `getMatchData` 得到 `source` 的 `[key, value]` 组合数组。

如果 `matchData` 的长度只有 `1` ，表明只有一个属性，并且 `matchData[0][2]` 为真值，则直接使用 `matchesStrictComparable` 创建一个比较函数。这样比使用 `baseIsMatch` 进行比较性能更加好。

否则返回一个函数，这个函数同样接收 `object` 作为参数，如果 `object === source` ，两者相等，肯定是匹配的，否则，调用 `baseIsMathch` 比较。

因此，本质上，`baseMatches` 就是 `baseIsMatch` 的偏函数。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

