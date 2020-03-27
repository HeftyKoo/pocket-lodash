# lodash源码分析之sampleSize

本文为读 lodash 源码的第一百六十三篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 依赖

```javascript
import copyArray from './.internal/copyArray.js'
import slice from './slice.js'
```

[《lodash源码分析之copyArray》](internal/copyArray.md)
[《读lodash源码之从slice看稀疏数组与密集数组》](slice.md)

## 源码分析

`sampleSize` 和 `sample` 的作用是类似的，不过 `sampleSize` 会随机返回 `array` 中指定数量 `n` 的元素。 

源码如下：

```javascript
function sampleSize(array, n) {
  n = n == null ? 1 : n
  const length = array == null ? 0 : array.length
  if (!length || n < 1) {
    return []
  }
  n = n > length ? length : n
  let index = -1
  const lastIndex = length - 1
  const result = copyArray(array)
  while (++index < n) {
    const rand = index + Math.floor(Math.random() * (lastIndex - index + 1))
    const value = result[rand]
    result[rand] = result[index]
    result[index] = value
  }
  return slice(result, 0, n)
}
```

### 参数合规化处理

如果没有传递 `n`，即没有指定随机的个数，则默认为 `1` 。

如果 `length` 为 `0` ，或者 `n < 1` ，也即传入的 `array` 为空数组或者 `null` 、`undefined`，或者要求的数量小于 `1` 时，都直接返回空数组。

接下来再判断指定的数量 `n` 是否超出来数组长度的范围，如果超出，则将 `n` 重置为 `length`，这很容易理解，随机的数量最多也只能跟 `array` 的长度一样。

### 思路

从 `sample` 中的分析我们知道，要从数组中取一个值，只需要取一个随机索引，然后再将该索引对应的值取出即可。那要怎样取一组值呢？

我们按照 `sample` 的方式，得到一个随机索引，并且将值取出，将这个值放到数组的第一位，再将第一位的值放到这个随机索引的位置。

第二次取随机索引的时候，因为第一位是已经取出来的值，因此随机索引要从第二位开始，再取出一个随机值，和第二位的值进行交换，直至 `n` 个数取出完成，这样，数组的 `0-n` 位都是随机取出来的值，只需要将这些值返回即可。

这里用 `index` 来表示当前已取出的随机值的索引，会不断递增，直至 `n` 个值取完。

因为会涉及数组元素的交互，会对数组进行更改，因此不能直接操作原数组，需要用 `copyArray` 对数组进行复制。

可以看到，计算随机索引的公式如下：

```javascript
index + Math.floor(Math.random() * (lastIndex - index + 1))
```

`lastIndex - index + 1` 其实就是数组除已取出的随机值外剩余的数量，对剩余的数量取随机值，然后再加上 `index` ，避免和已经取出的索引重叠。

计算得出随机索引后，后面的代码其实就是值的交互。

最后使用 `slice` 将 `0-n` 的随机值取出即可。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 