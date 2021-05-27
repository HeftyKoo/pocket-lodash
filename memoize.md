# lodash源码分析之memoize

本文为读 lodash 源码的第六十六篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 源码分析

`memoize` 的作用是缓存传入函数的计算结果，这个对于重复执行计算量繁重的函数相当有用。

源码如下：

```javascript
function memoize(func, resolver) {
  if (typeof func !== 'function' || (resolver != null && typeof resolver !== 'function')) {
    throw new TypeError('Expected a function')
  }
  const memoized = function(...args) {
    const key = resolver ? resolver.apply(this, args) : args[0]
    const cache = memoized.cache

    if (cache.has(key)) {
      return cache.get(key)
    }
    const result = func.apply(this, args)
    memoized.cache = cache.set(key, result) || cache
    return result
  }
  memoized.cache = new (memoize.Cache || Map)
  return memoized
}

memoize.Cache = Map
```

可以看到，`memoize` 接收两个参数，第一个参数 `func` 即为要执行计算的函数，可选参数 `resolver` 可以返回一个值作为缓存结果的 `key` 。

`memoize` 最终返回的结果也一个函数 `memoized`，在使用原函数 `func` 的地方，替换成这个返回函数的调用，就会获得结果缓存的能力。

### 参数合规性检测

```javascript
if (typeof func !== 'function' || (resolver != null && typeof resolver !== 'function')) {
  throw new TypeError('Expected a function')
}
```

这段是检测传入的 `func` 必须为函数，如果 `resolve` 也有传，也必须要传入函数。

 ### 获取缓存的 `key`

```javascript
const key = resolver ? resolver.apply(this, args) : args[0]
```

如果有传 `resolver` 函数，则会将所有参数都传给 `resolver` 函数，由 `resolver` 函数根据参数来计算出缓存的 `key` 。

如果没有传 `resolver` 函数，则默认取第一个参数作为缓存的 `key` 。

### 设置缓存

```javascript
const cache = memoized.cache
const result = func.apply(this, args)
memoized.cache = cache.set(key, result) || cache

memoize.Cache = Map
```

可以看到，缓存直接存储在 `memoized` 的 `cache` 字段中，并且默认使用 `Map` 来存储。

使用者也可以使用其他的缓存方式，直接将构造函数赋值给 `memoize.Cache` 即可，但是必须具备和 `Map` 一样的方法。

在函数第一次调用，或者在参数改动，造成缓存的 `key` 第一次出现的时候，肯定是没有缓存的，这时 `memoized` 会调用原函数 `func` 得到计算结果，并且将计算结果存储起来。

### 使用缓存

```javascript
if (cache.has(key)) {
  return cache.get(key)
}
```

缓存的使用其实就是判断缓存池里有没有，如果有，就直接返回结果，不再执行原函数。

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面 