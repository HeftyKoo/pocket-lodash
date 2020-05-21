# lodash源码分析之createCaseFirst

本文为读 lodash 源码的第三百一十篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import castSlice from './castSlice.js'
import hasUnicode from './hasUnicode.js'
import stringToArray from './stringToArray.js'
```

[lodash源码分析之castSlice](./castSlice.md)

[lodash源码分析之hasUnicode](./hasUnicode.md)

[lodash源码分析之stringToArray](./stringToArray.md)


## 源码分析

`createCaseFirst` 用来创建 `lowerFirst` 、`upperFirst` 之类的方法，即对字符串的第一个字符进行处理。

这个方法的思路是，将字符串的第一个字符取出，然后调用字符串对应的 `toLowerCase` 、`toUpperCase` 之类的方法，来对第一个字符进行转换，然后再和余下的字符拼接在一起。

有了这个思路，很容易写出这样的代码：

```javascript
function createCaseFirst(methodName) {
  return (string) => {
    if (!string) {
      return ''
    }
		
    const chr = string[0]
    const trailling = string.slice(1)
    
    return chr[methodName]() + trailling
  }
}
```

如果都是英文，这是没有问题，但是如果字符串里有 `unicode` 编码，就有可能造成乱码。

`lodash` 的源码如下：
```javascript
function createCaseFirst(methodName) {
  return (string) => {
    if (!string) {
      return ''
    }

    const strSymbols = hasUnicode(string)
      ? stringToArray(string)
      : undefined

    const chr = strSymbols
      ? strSymbols[0]
      : string[0]

    const trailing = strSymbols
      ? castSlice(strSymbols, 1).join('')
      : string.slice(1)

    return chr[methodName]() + trailing
  }
}
```

如果传入的字符串为假值，则返回一个空字符串。

如果字符串中有 `unicode` 编码，则调用 `unicodeToArray` 将字符串转换成数组 `strSymbols` ，这个方法不会造成 `unicode` 编码的字符乱码。

如果是含有 `unicode` 编码的字符，则从数组中取出第一项，否则直接从 `string` 中取出第一个字符。

余下的字符也同理，如果含有 `unicode` 编码，因为 `strSymbols` 是数组，因此调用 `castSlice` 来得到除了第一项外的所有元素，再调用数组的 `join` 方法拼成字符串，如果没有 `unicode` 编码，则直接调用字符串的 `slice` 方法，得到除了第一个字符外余下的字符串。

最后，调用字符串上对应的 `methodName` ，转换第一个字符，再和余下的字符串拼接起来，这样就将字符串中的第一个字符进行了转换。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 