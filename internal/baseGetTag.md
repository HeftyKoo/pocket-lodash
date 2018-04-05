# lodash源码分析之获取数据类型

> 所有的悲伤，总会留下一丝欢乐的线索，所有的遗憾，总会留下一处完美的角落，我在冰峰的深海，寻找希望的缺口，却在惊醒时，瞥见绝美的阳光！
>
> ——几米

本文为读 lodash 源码的第十八篇，后续文章会更新到这个仓库中，欢迎 star：[pocket-lodash](https://github.com/yeyuqiudeng/pocket-lodash)

gitbook也会同步仓库的更新，gitbook地址：[pocket-lodash](https://www.gitbook.com/book/yeyuqiudeng/pocket-lodash/details)

## 作用与用法

我们都知道，可以借用 `Object` 原型上的 `toString` 方法来获取数据的类型。 `baseGetTag` 利用的也是这一特性，其返回的结果如 `[object String]` 这样的形式，调用方式如下：

```javascript
baseGetTag('string') // [object String] 
```

## 为什么可以用Object.prototype.toString

先看 `es5` 规范对 `Object.prototyep.toString` 的运行步骤规定：

> 当调用 toString 方法，采用如下步骤：
>
> 1. 如果 this 的值是 undefined, 返回 "[object Undefined]".
> 2. 如果 this 的值是 null, 返回 "[object Null]".
> 3. 令 O 为以 this 作为参数调用 ToObject 的结果 .
> 4. 令 class 为 O 的 [[Class]] 内部属性的值 .
> 5. 返回三个字符串 "[object ", class, and "]" 连起来的字符串 .

在第三步的时候，会调用 `ToObject` 来转换成对象，而转换成对象后，会有个 `[[Class]]` 的内部属性，而这个内部属性的值正是 `toString` 的关键部分。

接下来再看规范对 `[[Class]]` 的规定：

> 本规范的每种内置对象都定义了 [[Class]] 内部属性的值。宿主对象的 [[Class]] 内部属性的值可以是除了 "Arguments", "Array", "Boolean", "Date", "Error", "Function", "JSON", "Math", "Number", "Object", "RegExp", "String" 的任何字符串。[[Class]] 内部属性的值用于内部区分对象的种类。注，本规范中除了通过 Object.prototype.toString ( 见 15.2.4.2) 没有提供任何手段使程序访问此值。

由规范可见，要获取这个 `[[Class]]` 内部属性的值的唯一手段是通过 `Object.prototype.toString` 。

## 源码分析

源码如下：

```javascript
const objectProto = Object.prototype
const hasOwnProperty = objectProto.hasOwnProperty
const toString = objectProto.toString
const symToStringTag = typeof Symbol != 'undefined' ? Symbol.toStringTag : undefined

function baseGetTag(value) {
  if (value == null) {
    return value === undefined ? '[object Undefined]' : '[object Null]'
  }
  if (!(symToStringTag && symToStringTag in Object(value))) {
    return toString.call(value)
  }
  const isOwn = hasOwnProperty.call(value, symToStringTag)
  const tag = value[symToStringTag]
  let unmasked = false
  try {
    value[symToStringTag] = undefined
    unmasked = true
  } catch (e) {}

  const result = toString.call(value)
  if (unmasked) {
    if (isOwn) {
      value[symToStringTag] = tag
    } else {
      delete value[symToStringTag]
    }
  }
  return result
}

export default baseGetTag
```

### Symbol.toStringTag

在 `ES6` 中，规范对 `Object.prototype.toString` 的步骤进行了重新定义，不再使用 `[[Class]]` 的内部属性进行获取，具体的规范如下:

> 在ES6，调用 `Object.prototype.toString` 时，会进行如下步骤：
>
> 1. 如果 `this` 是 `undefined` ，返回 `'[object Undefined]'` ;
> 2. 如果 `this` 是 `null` , 返回 `'[object Null]'` ；
> 3. 令 `O` 为以 `this` 作为参数调用 `ToObject` 的结果；
> 4. 令 `isArray` 为 `IsArray(O)` ；
> 5. `ReturnIfAbrupt(isArray)` （如果 `isArray` 不是一个正常值，比如抛出一个错误，中断执行）；
> 6. 如果 `isArray` 为 `true` ， 令 `builtinTag` 为 `'Array'` ;
> 7. `else` ，如果 `O is an exotic String object` ， 令 `builtinTag` 为 `'String'` ；
> 8. `else` ，如果 `O` 含有 `[[ParameterMap]] internal slot,` ， 令 `builtinTag` 为 `'Arguments'` ；
> 9. `else` ，如果 `O` 含有 `[[Call]] internal method` ， 令 `builtinTag` 为 `Function` ；
> 10. `else` ，如果 `O` 含有 `[[ErrorData]] internal slot` ， 令 `builtinTag` 为 `Error` ；
> 11. `else` ，如果 `O` 含有 `[[BooleanData]] internal slot` ， 令 `builtinTag` 为 `Boolean` ；
> 12. `else` ，如果 `O` 含有 `[[NumberData]] internal slot` ， 令 `builtinTag` 为 `Number` ；
> 13. `else` ，如果 `O` 含有 `[[DateValue]] internal slot` ， 令 `builtinTag` 为 `Date` ；
> 14. `else` ，如果 `O` 含有 `[[RegExpMatcher]] internal slot` ， 令 `builtinTag` 为 `RegExp` ；
> 15. `else` ， 令 `builtinTag` 为 `Object` ；
> 16. 令 `tag` 为 `Get(O, @@toStringTag)` 的返回值（ `Get(O, @@toStringTag)` 方法，既是在 `O` 是一个对象，并且具有 `@@toStringTag` 属性时，返回 `O[Symbol.toStringTag]` ）；
> 17. `ReturnIfAbrupt(tag)` ，如果 `tag` 是正常值，继续执行下一步；
> 18. 如果 `Type(tag)` 不是一个字符串，`let tag be builtinTag` ；
> 19. 返回由三个字符串 `"[object", tag, and "]"` 拼接而成的一个字符串。

规范对类型的判断进行了细化，前15步可以看成跟 `es5` 的作用一样，获取到数据的类型 `builtinTag` ，但是第16步调用了 `@@toStringTag` 的方法，如果再看规范的描述，可以知道这个其实是对象中的 `Symbol.toStringTag` 属性，如果这个属性返回的是一个字符串，则采用这个返回值 `tag` 作为数据的类型，否则才采用 `builtinTag` 。

### 处理null和undefined

```javascript
if (value == null) {
  return value === undefined ? '[object Undefined]' : '[object Null]'
}
```

这里是处理浏览器兼容性，在 `es5` 之前，并没有对 `null` 和 `undefined` 进行处理，所以返回的都是 `[object Object]` 。

### 处理不含Symbol.toStringTag的情况

```javascript
if (!(symToStringTag && symToStringTag in Object(value))) {
   return toString.call(value)
}
```

如果浏览器不支持 `Symbol` 或者 `value` 并不存在 `Symbol.toStringTag` 的方法，则可以直接调用 `toString` ，将结果返回了。

### 处理Symbol.toStringTag 的情况

```javascript
const isOwn = hasOwnProperty.call(value, symToStringTag)
const tag = value[symToStringTag]
let unmasked = false
try {
  value[symToStringTag] = undefined
  unmasked = true
} catch (e) {}

const result = toString.call(value)
if (unmasked) {
  if (isOwn) {
    value[symToStringTag] = tag
  } else {
    delete value[symToStringTag]
 }
}
```

 为了避免 `Symbol.toStringTag` 的影响，先将 `value` 的 `Symbol.toStringTag` 设置为 `undefined` ，这样可以屏蔽掉原型链上的 `Symbol.toStringTag` 属性，然后再使用 `toString` 方法获取到 `value` 的属性描述。

在获取到属性描述后，如果 `Symbol.toStringTag` 为自身的属性（不为原型链上的属性），则将原来保存下来的 `tag` 重新赋值，否则将 `Symbol.toStringTag` 属性移除。

## 参考

[es5规范中文版](http://yanhaijing.com/es5/#640)

[Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/#sec-completion-record-specification-type)

[MDN:Symbol.toStringTag](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/toStringTag)

[ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/symbol#Symbol-toStringTag)

[谈谈 Object.prototype.toString 。](https://github.com/jkchao/blog/issues/8)

## License

[署名-非商业性使用-禁止演绎 4.0 国际 (CC BY-NC-ND 4.0)](http://creativecommons.org/licenses/by-nc-nd/4.0/)

最后，所有文章都会同步发送到微信公众号上，欢迎关注,欢迎提意见：  ![](https://raw.githubusercontent.com/yeyuqiudeng/resource/master/images/qrcode_front-end-article.jpg) 

作者：对角另一面