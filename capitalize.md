# lodash源码分析之capitalize

本文为读 lodash 源码的第三百一十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import upperFirst from './upperFirst.js'
import toString from './toString.js'
```

[《lodash源码分析之upperFirst》](./upperFirst.md)

[《lodash源码分析之toString》](./toString.md)

## 源码分析

`capitalize` 会将字符串的第一个字母变大写，除第一个字母外其它变成小写。

源码如下：

```javascript
const capitalize = (string) => upperFirst(toString(string).toLowerCase())
```

其实就是先将所有的字符都先转换成小写，然后调用 `upperFirst` 将第一个字符变成大写。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

