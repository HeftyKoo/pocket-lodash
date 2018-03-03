# lodash源码分析之map的实现

> 宗教与哲学的分野，一个是信仰，一个是怀疑。宗教，稍有怀疑，就被视为异端。
>
> ——木心《文学回忆录》

本文为读 lodash 源码的第十五篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

```javascript
function map(array, iteratee) {
  let index = -1
  const length = array == null ? 0 : array.length
  const result = new Array(length)

  while (++index < length) {
    result[index] = iteratee(array[index], index, array)
  }
  return result
}
```

`lodash` 的 `map` 函数实现很简单，了解 `map` 用法的应该不难理解。

`map` 用来实现映射，返回新的数组长度应该跟原数组长度一致，因此一开始就先拿到原数组的 `length` 属性，创建一个跟原数组长度一致的空数组。

接着遍历，将数组中的每项依次取出，作为第一个参数传递给 `iteratee` 处理函数。`iteratee` 的第二个参数为当前项的索引，第三个参数为原数组。然后将 `iteratee` 返回的结果放入新数组 `result` 中。

遍历完毕后，将新的数组返回，即实现了 `map` 函数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面