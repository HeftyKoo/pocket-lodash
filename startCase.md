# lodash源码分析之startCase

本文为读 lodash 源码的第三百三十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import words from './words.js'
import toString from './toString.js'
```

[《lodash源码分析之words》](./words.md)

[《lodash源码分析之toString》](./toString.md)

## 源码分析

`startCase` 用来将字符串转换成单词，每个单词用空格隔开，并且每个单词的首字母大写。

如将 `foo-Bar` 转换成 `Foo Bar` 。

源码如下：

```javascript
const startCase = (string) => (
  words(`${string}`.replace(/['\u2019]/g, '')).reduce((result, word, index) => (
    result + (index ? ' ' : '') + upperFirst(word)
  ), '')
)
```

同样，先将 `\u2019` 即 `’` 符号移除，然后调用 `words` 将字符串分成单词数组。

然后调用数组的 `reduce` 方法遍历单词，对每个单词都调用 `upperFirst` 方法全部转换成首字母大写。如果 `index` 为真值，即不为第一个单词，则用空格将其与之前的结果连接在一起。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

