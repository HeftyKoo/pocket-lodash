# lodash源码分析之repeat

本文为读 lodash 源码的第三百二十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)


## 源码分析

`repeat` 的作用是将指定的字符串重复 `n` 次生成一个新的字符串。

其实可以很简单地就完成这个功能：

```javascript
function repeat (string, n) {
  let result = ''
  for (let i = 0; i < n; i++) {
    result += string
  }
  return result
}
```

但是这样实现性能并不是最好的，来看看 `lodash` 的实现：

```javascript
function repeat(string, n) {
  let result = ''
  if (!string || n < 1 || n > Number.MAX_SAFE_INTEGER) {
    return result
  }
 
  do {
    if (n % 2) {
      result += string
    }
    n = Math.floor(n / 2)
    if (n) {
      string += string
    }
  } while (n)

  return result
}
```

首先做一些不合规的参数处理，如果传入的字符串为假值，或者 `n < 1` 或者 `n` 比最大安全整数要大，则直接返回一个空的字符串。

接下来的算法思路，其实是每次都累加次一前的重复的结果，例如传入的是 `ab` ，第一次重复是 `abab` ，第二次重复就是 `abababab` ，相当于 `repeat(repeat(2), 2)` ，其实就是对 `2` 的幂次方相加。

但是传入的数字可能不为 `2` 的幂次方，因此每次计算的时候，都要判断是否需要将之前的结果相加，再进行翻倍。

## License 

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 

