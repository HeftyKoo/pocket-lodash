# lodash源码分析之lowerCase

本文为读 lodash 源码的第三百二十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import words from './words.js'
import toString from './toString.js'
```

[《lodash源码分析之words》](./words.md)

[《lodash源码分析之toString》](./toString.md)

## 源码分析

`lowerCase` 的方法是将字符串转换成小写。和原生的 `toLowerCase` 方法不一样的地方在于，`lowerCase` 会对字符串进行分词，每个单词用空格连接，特殊字符会被移除。

源码如下：

```javascript
const reQuotes = /['\u2019]/g
const lowerCase = (string) => (
  words(toString(string).replace(reQuotes, '')).reduce((result, word, index) => (
    result + (index ? ' ' : '') + word.toLowerCase()
  ), '')
)

```

同样，先将 `\u2019` 即 `’` 符号移除，然后调用 `words` 将字符串分成单词数组。

然后调用数组的 `reduce` 方法遍历单词，对每个单词都调用 `toLowerCase` 方法全部转换成小写。如果 `index` 为真值，即不为第个单词，则用空格将其与之前的结果连接在一起。

当遍历完毕后，所有单词都用空格连接在一起，并且都转换成小写了。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

