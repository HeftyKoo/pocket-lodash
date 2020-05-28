# lodash源码分析之replace

本文为读 lodash 源码的第三百三十一篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)


## 源码分析

`replace` 和字符串的 `replace` 方法作用一样，没有其它特别的地方。

源码如下:

```javascript
function replace(...args) {
  const string = `${args[0]}`
  return args.length < 3 ? string : string.replace(args[1], args[2])
}
```

可以传入三个参数，第一个是字符串，第二个是匹配模式，第三个是要替换的字符串。

如果参数小于三个，不做任何转换，返回原来的字符串。

否则调用字符串的 `replace` 方法来替换。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

