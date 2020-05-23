# lodash源码分析之camelCase

本文为读 lodash 源码的第三百一十四篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import upperFirst from './upperFirst.js'
import words from './words.js'
import toString from './toString.js'
```

[《lodash源码分析之upperFirst》](./upperFirst.md)

[《lodash源码分析之words》](./words.md)

[《lodash源码分析之toString》](./toString.md)

## 源码分析

`camelCase` 用来将字符串转换成驼峰式。

源码如下：

```javascript
const camelCase = (string) => (
  words(toString(string).replace(/['\u2019]/g, '')).reduce((result, word, index) => {
    word = word.toLowerCase()
    return result + (index ? upperFirst(word) : word)
  }, '')
)
```

先调用 `toString` 将传入的参数都转换成字符串类型，然后调用 `replace` 方法，将 `\u2019` 即 `’` 符号移除，即像 `don’t` 这样的单词会被转换成 `dont` ，不会分拆成 `don T` 这样的形式。

接着调用 `words` 单词数组，然后调用 `reduce` 方法遍历及拼接。

在 `reduce` 过程中，会依次得到数组中的单词，先调用 `toLowerCase` 方法，将单词都转换成小写，驼峰式的写法规定第一个单词的第一个字母是小写，其它单词的第一个字母需要大写。

因此判断 `index` 是否为真值，如果是真值，表示比 `0` 大，即不为第一个单词，调用 `upperFirst` 将其第一个字母转换成大写，然后拼接起来即可。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

