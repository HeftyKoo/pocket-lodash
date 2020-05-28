# lodash源码分析之snakeCase

本文为读 lodash 源码的第三百三十二篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import words from './words.js'
import toString from './toString.js'
```

[《lodash源码分析之words》](./words.md)

[《lodash源码分析之toString》](./toString.md)

## 源码分析

`kebabCase` 用来将字符串转换成用 `_` 号连词的形式。

如将 `fooBar` 转换成 `foo_bar` 。

源码如下：

```javascript
const snakeCase = (string) => (
  words(toString(string).replace(/['\u2019]/g, '')).reduce((result, word, index) => (
    result + (index ? '_' : '') + word.toLowerCase()
  ), '')
)
```

同样，先将 `\u2019` 即 `’` 符号移除，然后调用 `words` 将字符串分成单词数组。

然后调用数组的 `reduce` 方法遍历单词，对每个单词都调用 `toLowerCase` 方法全部转换成小写。如果 `index` 为真值，即不为第个单词，则用 `_` 将其与之前的结果连接在一起。

当遍历完毕后，所有单词都用 `_` 连接在一起了。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

