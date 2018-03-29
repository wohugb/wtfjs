# 👀 屌丝列表

## `[]` 等于 `![]`

数组不等于数组

```js
[] == ![] // -> true
```

??? note "💡 说明"

    抽象相等运算符将双方转换为数字进行比较，双方由于不同的原因成为数字“0”。
    数组是真的, 所以在右边, 与真值相反的是`false`, 然后被强制`0`.
    在左边, 然而, 一个空数组被强制为一个数字，而不是先成为布尔值, 并且空阵列被强制`0`, 尽管是真的.

    这是这个表达式如何简化

    ```js
    +[] == +![]
    0 == +false
    0 == 0
    true
    ```

    也可以看看[`[]` 是真的, 但不是`true`](#-is-truthy-but-not-true).

    * [**12.5.9** 逻辑NOT运算符 (`!`)](https://www.ecma-international.org/ecma-262/#sec-logical-not-operator)
    * [**7.2.13** 抽象的平等比较](https://www.ecma-international.org/ecma-262/#sec-abstract-equality-comparison)

## true 是 false

```js
!!'false' ==  !!'true'  // -> true
!!'false' === !!'true' // -> true
```

??? note "💡 说明"

    考虑这一步一步：

    ```js
    // true is 'truthy' and represented by value 1 (number), 'true' in string form is NaN.
    true == 'true'    // -> false
    false == 'false'  // -> false

    // 'false' is not the empty string, so it's a truthy value
    !!'false' // -> true
    !!'true'  // -> true
    ```

    * [**7.2.13** 抽象平等比较](https://www.ecma-international.org/ecma-262/#sec-abstract-equality-comparison)

## baNaNa

```js
'b' + 'a' + + 'a' + 'a'
```

这是JavaScript中的一个老派玩笑，但重新修复。
这是原来的一个：

```js
'foo' + + 'bar' // -> 'fooNaN'
```

??? note "💡 说明"

    The expression is evaluated as `'foo' + (+'bar')`, which converts `'bar'` to not a number.

    * [**12.8.3** The Addition Operator (`+`)](https://www.ecma-international.org/ecma-262/#sec-addition-operator-plus)
    * [12.5.6 Unary + Operator](https://www.ecma-international.org/ecma-262/#sec-unary-plus-operator)

## `NaN` is not a `NaN`

```js
NaN === NaN // -> false
```

??? note "💡 说明"

    规范严格定义了这种行为背后的逻辑：

    > 1. If `Type(x)` is different from `Type(y)`, return **false**.
    > 2. If `Type(x)` is Number, then
    >     1. If `x` is **NaN**, return **false**.
    >     2. If `y` is **NaN**, return **false**.
    >     3. … … …
    >
    > &mdash; [**7.2.14** Strict Equality Comparison](https://www.ecma-international.org/ecma-262/#sec-strict-equality-comparison)

    遵循IEEE的“NaN”定义：

    > 四种相互排斥的关系是可能的: 少于, 等于, 大于，无序.最后一种情况出现在至少一个操作数是NaN的情况下。每个NaN都会将无序与包括它自己在内的所有事物进行比较。
    >
    > &mdash; [“IEEE754 NaN值返回false的所有比较的基本原理是什么？”](https://stackoverflow.com/questions/1565164/1573715#1573715) at StackOverflow

## 这是一个失败

你不会相信，但...

```js
(![]+[])[+[]]+(![]+[])[+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]
// -> 'fail'
```

??? note "💡 说明"

    By breaking that mass of symbols into pieces, we notice that the following pattern occurs often:

    ```js
    (![]+[]) // -> 'false'
    ![]      // -> false
    ```

    So we try adding `[]` to `false`.
    But due to a number of internal function calls (`binary + Operator` -> `ToPrimitive` -> `[[DefaultValue]]`) we end up converting the right operand to a string:

    ```js
    (![]+[].toString()) // 'false'
    ```

    Thinking of a string as an array we can access its first character via `[0]`:

    ```js
    'false'[0] // -> 'f'
    ```

    The rest is obvious, but the `i` is tricky.
    The `i` in `fail` is grabbed by generating the string `'falseundefined'` and grabbing the element on index `['10']`

## `[]` 是真的, 但不是`true`

一个数组是一个真值，然而，它不等于`true`。

```js
!![]       // -> true
[] == true // -> false
```

??? note "💡 说明"

    以下是ECMA-262规范中相应章节的链接：

    * [**12.5.9** 逻辑NOT运算符 (`!`)](https://www.ecma-international.org/ecma-262/#sec-logical-not-operator)
    * [**7.2.13** 抽象的平等比较](https://www.ecma-international.org/ecma-262/#sec-abstract-equality-comparison)

## `null`是假的, 但不是`false`

尽管`null`是一个虚假值，它不等于`false`。

```js
!!null        // -> false
null == false // -> false
```

同时，其他的假值，如`0`或`''`等于`false`。

```js
0 == false  // -> true
'' == false // -> true
```

??? note "💡 说明"

    解释与前面的例子相同。
    以下是相应的链接：

    * [**7.2.13** 抽象的平等比较](https://www.ecma-international.org/ecma-262/#sec-abstract-equality-comparison)

## `document.all` 是`Object`, 但它是`undefined`

> ⚠️ 这是浏览器API的一部分，不能在Node.js环境中使用 ⚠️

尽管`document.all`是一个类似数组的对象，它可以访问页面中的DOM节点, 使用`typeof`函数时返回`undefined`.

```js
document.all instanceof Object // -> true
typeof document.all // -> 'undefined'
```

同时，`document.all`不等于`undefined`。

```js
document.all === undefined // -> false
document.all === null // -> false
```

但在同时：

```js
document.all == null // -> true
```

??? note "💡 说明"

    > `document.all` used to be a way to access DOM elements, in particular with old versions of IE.
    While it has never been a standard it was broadly used in the old age JS code.
    When the standard progressed with new APIs (such as `document.getElementById`) this API call became obsolete and the standard commitee had to decide what to do with it.
    Because of its broad use they decided to keep the API but introduce a willful violation of the JavaScript specification.
    > The reason why it responds to `false` when using the [Strict Equality Comparison](https://www.ecma-international.org/ecma-262/#sec-strict-equality-comparison) with `undefined` while `true` when using the [Abstract Equality Comparison](https://www.ecma-international.org/ecma-262/#sec-abstract-equality-comparison) is due to the willful violation of the specification that explicitly allows that.
    >
    > &mdash; [“Obsolete features - document.all”](https://html.spec.whatwg.org/multipage/obsolete.html#dom-document-all) at WhatWG - HTML spec
    > &mdash; [“Chapter 4 - ToBoolean - Falsy values”](https://github.com/getify/You-Dont-Know-JS/blob/0d79079b61dad953bbfde817a5893a49f7e889fb/types%20%26%20grammar/ch4.md#falsy-objects) at YDKJS - Types & Grammar

## 最小值大于零

`Number.MIN_VALUE`是最小的数字, 这大于零:

```js
Number.MIN_VALUE > 0 // -> true
```

??? note "💡 说明"

    > `Number.MIN_VALUE` is `5e-324`, i.e. the smallest positive number that can be represented within float precision, i.e. that's as close as you can get to zero. It defines the best resolution that floats can give you.
    >
    > Now the overall smallest value is `Number.NEGATIVE_INFINITY` although it's not really numeric in a strict sense.
    >
    > &mdash; [“Why is `0` less than `Number.MIN_VALUE` in JavaScript?”](https://stackoverflow.com/questions/26614728/why-is-0-less-than-number-min-value-in-javascript) at StackOverflow

    * [**20.1.2.9** Number.MIN_VALUE](https://www.ecma-international.org/ecma-262/#sec-number.min_value)

## 函数不是一个函数

> ⚠️ V8 v5.5或更低版本中存在的错误（Node.js <= 7） ⚠️

你们所有人都知道烦人的 _`undefined`不是函数_ ，但是这个怎么样？

```js
// 声明一个扩展null的类
class Foo extends null {}
// -> [Function: Foo]

new Foo instanceof null
// > TypeError：函数不是函数
// >     at … … …
```

??? note "💡 说明"

    这不是规范的一部分。
    这只是一个现在已经修复的错误，所以在将来不应该有问题。

## 添加数组

如果你尝试添加两个数组呢？

```js
[1, 2, 3] + [4, 5, 6]  // -> '1,2,34,5,6'
```

??? note "💡 说明"

    串接发生。
    一步一步，它看起来像这样：

    ```js
    [1, 2, 3] + [4, 5, 6]
    // call toString()
    [1, 2, 3].toString() + [4, 5, 6].toString()
    // concatenation
    '1,2,3' + '4,5,6'
    // ->
    '1,2,34,5,6'
    ```

## 数组中的尾随逗号

你已经创建了一个有4个空元素的数组
尽管如此，你会得到一个包含三个元素的数组，因为尾随逗号：

```js
let a = [,,,]
a.length     // -> 3
a.toString() // -> ',,'
```

??? note "💡 说明"

    > **Trailing commas** (sometimes called "final commas") can be useful when adding new elements, parameters, or properties to JavaScript code.
    If you want to add a new property, you can simply add a new line without modifying the previously last line if that line already uses a trailing comma.
    This makes version-control diffs cleaner and editing code might be less troublesome.
    >
    > &mdash; [Trailing commas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas) at MDN

## 数组相等是一个怪物

数组相等是JS中的怪物，如下所示：

```js
[] == ''   // -> true
[] == 0    // -> true
[''] == '' // -> true
[0] == 0   // -> true
[0] == ''  // -> false
[''] == 0  // -> true

[null] == ''      // true
[null] == 0       // true
[undefined] == '' // true
[undefined] == 0  // true

[[]] == 0  // true
[[]] == '' // true

[[[[[[]]]]]] == '' // true
[[[[[[]]]]]] == 0  // true

[[[[[[ null ]]]]]] == 0  // true
[[[[[[ null ]]]]]] == '' // true

[[[[[[ undefined ]]]]]] == 0  // true
[[[[[[ undefined ]]]]]] == '' // true
```

??? note "💡 说明"

    你应该仔细观察上面的例子！
    该规范的[** 7.2.13 ** Abstract Equality Comparison](https://www.ecma-international.org/ecma-262/#sec-abstract-equality-comparison)部​​分描述了该行为。

## `undefined` 和 `Number`

如果我们不将任何参数传递给`Number`构造函数，我们就会得到`0`。
当没有实际参数时`undefined`被赋值给形式参数, 所以你可能期望没有参数的`Number`将`undefined`作为其参数的值。
但是，当我们传递`undefined`时，我们会得到`NaN`。

```js
Number()          // -> 0
Number(undefined) // -> NaN
```

??? note "💡 说明"

    根据具体情况:

    1. 如果没有参数传递给这个函数的调用，让`n`为`+ 0`。
    2. 否则，让`n`成为 ? `ToNumber(value)`。
    3. `undefined`的情况下, `ToNumber(undefined)`应该返回`NaN`.

    这是相应的部分:

    * [**20.1.1** 数字构造器](https://www.ecma-international.org/ecma-262/#sec-number-constructor)
    * [**7.1.3** ToNumber(`argument`)](https://www.ecma-international.org/ecma-262/#sec-tonumber)

## `parseInt`是个坏人

`parseInt`以其怪癖闻名于世:

```js
parseInt('f*ck');     // -> NaN
parseInt('f*ck', 16); // -> 15
```

??? note "💡 说明"

    发生这种情况是因为`parseInt`将继续逐个字符地解析，直到遇到一个不知道的字符。
    `f * ck`中的`f`是十六进制数字`15`。

解析`Infinity`为整数是...

```js
//
parseInt('Infinity', 10) // -> NaN
// ...
parseInt('Infinity', 18) // -> NaN...
parseInt('Infinity', 19) // -> 18
// ...
parseInt('Infinity', 23) // -> 18...
parseInt('Infinity', 24) // -> 151176378
// ...
parseInt('Infinity', 29) // -> 385849803
parseInt('Infinity', 30) // -> 13693557269
// ...
parseInt('Infinity', 34) // -> 28872273981
parseInt('Infinity', 35) // -> 1201203301724
parseInt('Infinity', 36) // -> 1461559270678...
parseInt('Infinity', 37) // -> NaN
```

解析`null`也要小心：

```js
parseInt(null, 24) // -> 23
```

??? note "💡 说明"

    > 它将`null`转换为字符串`"null"`并尝试转换它。对于基数0到23，没有可以转换的数字，所以它返回NaN。在24，`"n"`，第14个字母，被添加到数字系统中。在31，添加第21个字母`"u"`，并且可以解码整个字符串。在37时，不再有任何可以生成的有效数字集并返回`NaN`。
    >
    > &mdash; [“parseInt(null, 24) === 23… wait, what?”](https://stackoverflow.com/questions/6459758/parseintnull-24-23-wait-what) at StackOverflow

不要忘记八分之一:

```js
parseInt('06'); // 6
parseInt('08'); // 8 if support ECMAScript 5
parseInt('08'); // 0 if not support ECMAScript 5
```

??? note "💡 说明"

    如果输入字符串以"0"开头，则基数为八（八进制）或十（十进制）。
    究竟选择哪个基数是依赖于实现的。
    ECMAScript 5指定使用10（十进制），但并非所有浏览器都支持此功能。
    出于这个原因，当使用`parseInt`时总是指定一个基数。

    `parseInt`总是将​​输入转换为字符串：

    ```js
    parseInt({ toString: () => 2, valueOf: () => 1 }) // -> 2
    Number({ toString: () => 2, valueOf: () => 1 })   // -> 1
    ```

### 数学用`true`和`false`

让我们来做一些数学：

```js
true + true // -> 2
(true + true) * (true + true) - true // -> 3
```

Hmmm… 🤔

??? note "💡 说明"

    We can coerce values to numbers with the `Number` constructor.
    It's quite obvious that `true` will be coerced to `1`:

    ```js
    Number(true) // -> 1
    ```

    The unary plus operator attempts to convert its value into a number.
    It can convert string representations of integers and floats, as well as the non-string values `true`, `false`, and `null`.
    If it cannot parse a particular value, it will evaluate to `NaN`.
    That means we can coerce `true` to `1` easier:

    ```js
    +true // -> 1
    ```

    When you're performing addition or multiplication, the `ToNumber` method is invoked.
    According to the specification, this method returns:

    > If `argument` is **true**, return **1**.
    If `argument` is **false**, return **+0**.

    That's why we can add boolean values as regular numbers and get correct results.

    Corresponding sections:

    * [**12.5.6** Unary `+` Operator](https://www.ecma-international.org/ecma-262/#sec-unary-plus-operator)
    * [**12.8.3** The Addition Operator (`+`)](https://www.ecma-international.org/ecma-262/#sec-addition-operator-plus)
    * [**7.1.3** ToNumber(`argument`)](https://www.ecma-international.org/ecma-262/#sec-tonumber)

## HTML注释在JavaScript中有效

您会留下深刻的印象，但`<!--`（这被称为HTML注释）是JavaScript中的有效注释。

```js
// valid comment
<!-- valid comment too
```

??? note "💡 说明"

    印象深刻？,类似HTML的注释旨在允许不理解`<script>`标签的浏览器优雅地降级。
    这些浏览器，例如,Netscape 1.x不再受欢迎。
    因此，将HTML注释放入脚本标记中实在没有意义。

    由于Node.js基于V8引擎，因此Node.js运行时也支持类似HTML的注释。
    而且，它们是规范的一部分：

    * [**B.1.3** 类似HTML的评论](https://www.ecma-international.org/ecma-262/#sec-html-like-comments)

## `NaN`~~不是~~一个数字

`NaN`的类型是`'number'`：

```js
typeof NaN            // -> 'number'
```

??? note "💡 说明"

    `typeof` and `instanceof`操作符的解释：

    * [**12.5.5** `typeof`运算符](https://www.ecma-international.org/ecma-262/#sec-typeof-operator)
    * [**12.10.4** 运行时语义: InstanceofOperator(`O`,`C`)](https://www.ecma-international.org/ecma-262/#sec-instanceofoperator)

## `[]`和`null`是对象

```js
typeof []   // -> 'object'
typeof null // -> 'object'

// 然而
null instanceof Object // false
```

??? note "💡 说明"

    The behavior of `typeof` operator is defined in this section of the specification:

    * [**12.5.5** The `typeof` Operator](https://www.ecma-international.org/ecma-262/#sec-typeof-operator)

    According to the specification, the `typeof` operator returns a string according to [Table 35: `typeof` Operator Results](https://www.ecma-international.org/ecma-262/#table-35).
    For `null`, ordinary, standard exotic and non-standard exotic objects, which do not implement `[[Call]]`, it returns the string `"object"`.

    However, you can check the type of an object by using the `toString` method.

    ```js
    Object.prototype.toString.call([])
    // -> '[object Array]'

    Object.prototype.toString.call(new Date)
    // -> '[object Date]'

    Object.prototype.toString.call(null)
    // -> '[object Null]'
    ```

## 神奇地增加数字

```js
999999999999999  // -> 999999999999999
9999999999999999 // -> 10000000000000000

10000000000000000       // -> 10000000000000000
10000000000000000 + 1   // -> 10000000000000000
10000000000000000 + 1.1 // -> 10000000000000002
```

??? note "💡 说明"

    This is caused by IEEE 754-2008 standard for Binary Floating-Point Arithmetic.
    At this scale, it rounds to the nearest even number.
    Read more:

    * [**6.1.6** The Number Type](https://www.ecma-international.org/ecma-262/#sec-ecmascript-language-types-number-type)
    * [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) on Wikipedia

## `0.1 + 0.2`的精度

A well-known joke.
An addition of `0.1` and `0.2` is deadly precise:

```js
0.1 + 0.2 // -> 0.30000000000000004
(0.1 + 0.2) === 0.3 // -> false
```

??? note "💡 说明"

    The answer for the [”Is floating point math broken?”](https://stackoverflow.com/questions/588004/is-floating-point-math-broken) question on StackOverflow:

    > The constants `0.2` and `0.3` in your program will also be approximations to their true values.
    It happens that the closest `double` to `0.2` is larger than the rational number `0.2` but that the closest `double` to `0.3` is smaller than the rational number `0.3`.
    The sum of `0.1` and `0.2` winds up being larger than the rational number `0.3` and hence disagreeing with the constant in your code.

    This problem is so known that there is even a website called [0.30000000000000004.com](http://0.30000000000000004.com/).
    It occurs in every language that uses floating-point math, not just JavaScript.

## 修补数字

您可以添加自己的方法来封装像`Number`或`String`这样的对象。

```js
Number.prototype.isOne = function () {
  return Number(this) === 1
}

1.0.isOne() // -> true
1..isOne()  // -> true
2.0.isOne() // -> false
(7).isOne() // -> false
```

??? note "💡 说明"

    很显然，你可以像使用JavaScript的其他对象一样扩展`Number`对象。
    但是，如果定义的方法的行为不是规范的一部分，则不推荐使用。
    这是`Number`属性的列表:

    * [**20.1** 数字对象](https://www.ecma-international.org/ecma-262/#sec-number-objects)

## 三个数字的比较

```js
1 < 2 < 3 // -> true
3 > 2 > 1 // -> false
```

??? note "💡 说明"

    Why does this work that way? Well, the problem is in the first part of an expression.
    Here's how it works:

    ```js
    1 < 2 < 3 // 1 < 2 -> true
    true  < 3 // true -> 1
    1     < 3 // -> true

    3 > 2 > 1 // 3 > 2 -> true
    true  > 1 // true -> 1
    1     > 1 // -> false
    ```

    We can fix this with _Greater than or equal operator (`>=`)_:

    ```js
    3 > 2 >= 1 // true
    ```

    Read more about Relational operators in the specification:

    * [**12.10** Relational Operators](https://www.ecma-international.org/ecma-262/#sec-relational-operators)

## 有趣的数学

Often the results of arithmetic operations in JavaScript might be quite unexpected.
Consider these examples:

```js
 3  - 1  // -> 2
 3  + 1  // -> 4
'3' - 1  // -> 2
'3' + 1  // -> '31'

'' + '' // -> ''
[] + [] // -> ''
{} + [] // -> 0
[] + {} // -> '[object Object]'
{} + {} // -> '[object Object][object Object]'

'222' - -'111' // -> 333

[4] * [4]       // -> 16
[] * []         // -> 0
[4, 4] * [4, 4] // NaN
```

??? note "💡 说明"

    What's happening in the first four examples? Here's a small table to understand addition in JavaScript:

    ```
    Number  + Number  -> addition
    Boolean + Number  -> addition
    Boolean + Boolean -> addition
    Number  + String  -> concatenation
    String  + Boolean -> concatenation
    String  + String  -> concatenation
    ```

    What about other examples? A `ToPrimitive` and `ToString` methods are being implicitly called for `[]` and `{}` before addition.
    Read more about evaluation process in the specification:

    * [**12.8.3** The Addition Operator (`+`)](https://www.ecma-international.org/ecma-262/#sec-addition-operator-plus)
    * [**7.1.1** ToPrimitive(`input` [,`PreferredType`])](https://www.ecma-international.org/ecma-262/#sec-toprimitive)
    * [**7.1.12** ToString(`argument`)](https://www.ecma-international.org/ecma-262/#sec-tostring)

## RegExps 扩展

Did you know you can add numbers like this?

```js
// Patch a toString method
RegExp.prototype.toString = function() {
  return this.source
}

/7/ - /5/ // -> 2
```

??? note "💡 说明"

  * [**21.2.5.10** get RegExp.prototype.source](https://www.ecma-international.org/ecma-262/#sec-get-regexp.prototype.source)

## 字符串不是`String`的实例

```js
'str' // -> 'str'
typeof 'str' // -> 'string'
'str' instanceof String // -> false
```

??? note "💡 说明"

    The `String` constructor returns a string:

    ```js
    typeof String('str')   // -> 'string'
    String('str')          // -> 'str'
    String('str') == 'str' // -> true
    ```

    Let's try with a `new`:

    ```js
    new String('str') == 'str' // -> true
    typeof new String('str')   // -> 'object'
    ```

    Object? What's that?

    ```js
    new String('str') // -> [String: 'str']
    ```

    More information about the String constructor in the specification:

    * [**21.1.1** The String Constructor](https://www.ecma-international.org/ecma-262/#sec-string-constructor)

## 用反引号调用函数

Let's declare a function which logs all params into the console:

```js
function f(...args) {
  return args
}
```

No doubt, you know you can call this function like this:

```js
f(1, 2, 3) // -> [ 1, 2, 3 ]
```

But did you know you can call any function with backticks?

```js
f`true is ${true}, false is ${false}, array is ${[1,2,3]}`
// -> [ [ 'true is ', ', false is ', ', array is ', '' ],
// ->   true,
// ->   false,
// ->   [ 1, 2, 3 ] ]
```

??? note "💡 说明"

    Well, this is not magic at all if you're familiar with _Tagged template literals_.
    In the example above, `f` function is a tag for template literal.
    Tags before template literal allow you to parse template literals with a function.
    The first argument of a tag function contains an array of string values.
    The remaining arguments are related to the expressions.
    例:

    ```js
    function template(strings, ...keys) {
      // do something with strings and keys…
    }
    ```

    This is the [magic behind](http://mxstbr.blog/2016/11/styled-components-magic-explained/) famous library called [💅 styled-components](https://www.styled-components.com/), which is popular in the React community.

    Link to the specification:

    * [**12.3.7** Tagged Templates](https://www.ecma-international.org/ecma-262/#sec-tagged-templates)

## Call call call

> Found by [@cramforce](http://twitter.com/cramforce)

```js
console.log.call.call.call.call.call.apply(a => a, [1, 2])
```

??? note "💡 说明"

    Attention, it could break your mind! Try to reproduce this code in your head: we're applying the `call` method using the `apply` method.
    Read more:

    * [**19.2.3.3** Function.prototype.call(`thisArg`, ...`args`)](https://www.ecma-international.org/ecma-262/#sec-function.prototype.call)
    * [**19.2.3.1 ** Function.prototype.apply(`thisArg`, `argArray`)](https://www.ecma-international.org/ecma-262/#sec-function.prototype.apply)

## `constructor`属性

```js
const c = 'constructor'
c[c][c]('console.log("WTF?")')() // > WTF?
```

??? note "💡 说明"

    Let's 考虑这一点 example step-by-step:

    ```js
    // Declare a new constant which is a string 'constructor'
    const c = 'constructor'

    // c is a string
    c // -> 'constructor'

    // Getting a constructor of string
    c[c] // -> [Function: String]

    // Getting a constructor of constructor
    c[c][c] // -> [Function: Function]

    // Call the Function constructor and pass
    // the body of new function as an argument
    c[c][c]('console.log("WTF?")') // -> [Function: anonymous]

    // And then call this anonymous function
    // The result is console-logging a string 'WTF?'
    c[c][c]('console.log("WTF?")')() // > WTF?
    ```

    An `Object.prototype.constructor` returns a reference to the `Object` constructor function that created the instance object.
    In case with strings it is `String`, in case with numbers it is `Number` and so on.

    * [`Object.prototype.constructor`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor) at MDN
    * [**19.1.3.1** Object.prototype.constructor](https://www.ecma-international.org/ecma-262/#sec-object.prototype.constructor)

## 对象作为对象属性的键

```js
{ [{}]: {} } // -> { '[object Object]': {} }
```

??? note "💡 说明"

    Why does this work so? Here we're using a _Computed property name_.
    When you pass an object between those brackets, it coerces object to a string, so we get the property key `'[object Object]'` and the value `{}`.

    We can make "brackets hell" like this:

    ```js
    ({[{}]:{[{}]:{}}})[{}][{}] // -> {}

    // structure:
    // {
    //   '[object Object]': {
    //     '[object Object]': {}
    //   }
    // }
    ```

    在这里阅读更多关于对象文字:

    * [Object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer) at MDN
    * [**12.2.6** Object Initializer](http://www.ecma-international.org/ecma-262/6.0/#sec-object-initializer)

## 使用`__proto__`访问原型

As we know, primitives don't have prototypes.
However, if we try to get a value of `__proto__` for primitives, we would get this:

```js
(1).__proto__.__proto__.__proto__ // -> null
```

??? note "💡 说明"

    This happens because when something doesn't have a prototype, it will be wrapped into a wrapper object using the `ToObject` method.
    So, step-by-step:

    ```js
    (1).__proto__ // -> [Number: 0]
    (1).__proto__.__proto__ // -> {}
    (1).__proto__.__proto__.__proto__ // -> null
    ```

    Here is more information about `__proto__`:

    * [**B.2.2.1** Object.prototype.__proto__](https://www.ecma-international.org/ecma-262/#sec-object.prototype.__proto__)
    * [**7.1.13** ToObject(`argument`)](https://www.ecma-international.org/ecma-262/#sec-toobject)

## ``` `${{Object}}` ```

What is the result of the expression below?

```js
`${{Object}}`
```

The answer is:

```js
// -> '[object Object]'
```

??? note "💡 说明"

    We defined an object with a property `Object` using _Shorthand property notation_:

    ```js
    { Object: Object }
    ```

    Then we've passed this object to the template literal, so the `toString` method calls for that object.
    That's why we get the string `'[object Object]'`.

    * [**12.2.9** Template Literals](https://www.ecma-international.org/ecma-262/#sec-template-literals)
    * [Object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer) at MDN

## 用默认值解构

考虑这个例子:

```js
let x, { x: y = 1 } = { x }; y;
```

上面的例子是面试的一项重要任务。
`y`的值是什么？,答案是：

```js
// -> 1
```

??? note "💡 说明"

    ```js
    let x, { x: y = 1 } = { x }; y;
    //  ↑       ↑           ↑    ↑
    //  1       3           2    4
    ```

    用上面的例子:

    1. 我们声明`x`没有值，所以它是`undefined`。
    2. 然后我们将`x`的值打包到对象属性`x`中。
    3. 然后我们使用解构来提取`x`的值，并且想把它赋值给`y`。 如果该值没有定义，那么我们将使用`1`作为默认值。
    4. 返回`y`的值。

    * MDN[对象初始化器](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)

## 点和扩展

有趣的例子可以用数组的扩展来组成。考虑这一点:

```js
[...[...'...']].length // -> 3
```

??? note "💡 说明"

    Why `3`? When we use the [spread operator](http://www.ecma-international.org/ecma-262/6.0/#sec-array-initializer), the `@@iterator` method is called, and the returned iterator is used to obtain the values to be iterated.
    The default iterator for string spreads a string into characters.
    After spreading, we pack these characters into an array.
    Then we spread this array again and pack it back to an array.

    A `'...'` string consists with three `.` characters, so the length of resulting array is `3`.

    Now, step-by-step:

    ```js
    [...'...']             // -> [ '.', '.', '.' ]
    [...[...'...']]        // -> [ '.', '.', '.' ]
    [...[...'...']].length // -> 3
    ```

    Obviously, we can spread and wrap the elements of an array as many times as we want:

    ```js
    [...'...']                 // -> [ '.', '.', '.' ]
    [...[...'...']]            // -> [ '.', '.', '.' ]
    [...[...[...'...']]]       // -> [ '.', '.', '.' ]
    [...[...[...[...'...']]]]  // -> [ '.', '.', '.' ]
    // and so on …
    ```

## 标签

没有多少程序员知道JavaScript中的标签。他们很有趣：

```js
foo: {
  console.log('first');
  break foo;
  console.log('second');
}

// > first
// -> undefined
```

??? note "💡 说明"

    带标签的语句与`break`或`continue`语句一起使用。
    您可以使用标签来标识循环，然后使用`break`或`continue`语句来指示程序是否应该中断循环或继续执行。

    在上面的例子中，我们标识了一个标签`foo`。
    之后`console.log('first');`执行，然后我们中断执行。

    阅读关于JavaScript中标签的更多信息：

    * [**13.13** 标记语句](https://tc39.github.io/ecma262/#sec-labelled-statements)
    * [标记语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label) at MDN

## 嵌套标签

```js
a: b: c: d: e: f: g: 1, 2, 3, 4, 5; // -> 5
```

??? note "💡 说明"

    与以前的例子类似，请按照这些链接:

    * [**12.16** 逗号运算符 (`,`)](https://www.ecma-international.org/ecma-262/#sec-comma-operator)
    * [**13.13** 标记语句](https://tc39.github.io/ecma262/#sec-labelled-statements)
    * [标记语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label) at MDN

## 阴险 `try..catch`

这个表达式会返回什么？? `2` 或者 `3`?

```js
(() => {
  try {
    return 2;
  } finally {
    return 3;
  }
})()
```

答案是 `3`.
诧异?

??? note "💡 说明"

    * [**13.15** `try` 声明](https://www.ecma-international.org/ecma-262/#sec-try-statement)

## 这是多重继承?

看看下面的例子:

```js
new (class F extends (String, Array) { }) // -> F []
```

这是一个多重继承? 不.

??? note "💡 说明"

    The interesting part is the value of the `extends` clause (`(String, Array)`).
    The grouping operator always returns its last argument, so `(String, Array)` is actually just `Array`.
    That means we've just created a class which extends `Array`.

    * [**14.5** Class Definitions](https://www.ecma-international.org/ecma-262/#sec-class-definitions)
    * [**12.16** Comma Operator (`,`)](https://www.ecma-international.org/ecma-262/#sec-comma-operator)

## A generator which yields itself

考虑这一点 example of a generator which yields itself:

```js
(function* f() { yield f })().next()
// -> { value: [GeneratorFunction: f], done: false }
```

As you can see, the returned value is an object with its `value` equal to `f`.
In that case, we can do something like this:

```js
(function* f() { yield f })().next().value().next()
// -> { value: [GeneratorFunction: f], done: false }

// and again
(function* f() { yield f })().next().value().next().value().next()
// -> { value: [GeneratorFunction: f], done: false }

// and again
(function* f() { yield f })().next().value().next().value().next().value().next()
// -> { value: [GeneratorFunction: f], done: false }

// and so on
// …
```

??? note "💡 说明"

    要理解为什么这样工作，请阅读规范的这些部分:

    * [**25** 控制抽象对象](https://www.ecma-international.org/ecma-262/#sec-control-abstraction-objects)
    * [**25.3** 生成器对象](https://www.ecma-international.org/ecma-262/#sec-generator-objects)

## A class of class

考虑这种混淆的语法播放:

```js
(typeof (new (class { class () {} }))) // -> 'object'
```

It seems like we're declaring a class inside of class.
Should be an error, however, we get the string `'object'`.

??? note "💡 说明"

    Since ECMAScript 5 era, _keywords_ are allowed as _property names_.
    So think about it as this simple object 例:

    ```js
    const foo = {
      class: function() {}
    };
    ```

    And ES6 standardized shorthand method definitions.
    Also, classes can be anonymous.
    So if we drop `: function` part, we're going to get:

    ```js
    class {
      class() {}
    }
    ```

    The result of a default class is always a simple object.
    And its typeof should return `'object'`.

    Read more here:

    * [**14.3** Method Definitions](https://www.ecma-international.org/ecma-262/#sec-method-definitions)
    * [**14.5** Class Definitions](https://www.ecma-international.org/ecma-262/#sec-class-definitions)

## Non-coercible objects

With well-known symbols, there's a way to get rid of type coercion.
Take a look:

```js
function nonCoercible(val) {
  if (val == null) {
    throw TypeError('nonCoercible should not be called with null or undefined')
  }

  const res = Object(val)

  res[Symbol.toPrimitive] = () => {
    throw TypeError('Trying to coerce non-coercible object')
  }

  return res
}
```

Now we can use this like this:

```js
// objects
const foo = nonCoercible({foo: 'foo'})

foo * 10      // -> TypeError: Trying to coerce non-coercible object
foo + 'evil'  // -> TypeError: Trying to coerce non-coercible object

// strings
const bar = nonCoercible('bar')

bar + '1'                 // -> TypeError: Trying to coerce non-coercible object
bar.toString() + 1        // -> bar1
bar === 'bar'             // -> false
bar.toString() === 'bar'  // -> true
bar == 'bar'              // -> TypeError: Trying to coerce non-coercible object

// numbers
const baz = nonCoercible(1)

baz == 1             // -> TypeError: Trying to coerce non-coercible object
baz === 1            // -> false
baz.valueOf() === 1  // -> true
```

??? note "💡 说明"

    * [A gist by Sergey Rubanov](https://gist.github.com/chicoxyzzy/5dd24608e886adf5444499896dff1197)
    * [**6.1.5.1** Well-Known Symbols](https://www.ecma-international.org/ecma-262/#sec-well-known-symbols)

## 棘手的箭头功能

考虑下面的例子:

```js
let f = () => 10
f() // -> 10
```

好，很好，但是这个怎么样？:

```js
let f = () => {}
f() // -> undefined
```

??? note "💡 说明"

    You might expect `{}` instead of `undefined`.
    This is because the curly braces are part of the syntax of the arrow functions, so `f` will return undefined.
    It is however possible to return the `{}` object directly from an arrow function, by enclosing the return value with brackets.

    ```js
    let f = () => ({})
    f() // -> {}
    ```

## `arguments`和箭头功能

考虑下面的例子:

```js
let f = function(){
  return arguments;
}
f('a'); // -> { '0': 'a' }
```

现在，尝试用箭头函数做同样的事情:

```js
let f = () =>  arguments;
f('a'); // -> Uncaught ReferenceError: arguments is not defined
```

??? note "💡 说明"

    Arrow functions are a lightweight version of regular functions with a focus on being short and lexical `this`.
    At the same time arrow functions do not provide a binding for the `arguments` object.
    As a valid alternative use the `rest parameters` to achieve the same result:

    ```js
    let f = (...args) => args;
    f('a');
    ```

    * [Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) at MDN.

## 棘手的`return`

`return`语句也很棘手.考虑这一点:

```js
(function () {
  return
  {
    b : 10
  }
})() // -> undefined
```

??? note "💡 说明"

    `return` and the returned expression must be in the same line:

    ```js
    (function () {
      return {
        b : 10
      }
    })() // -> { b: 10 }
    ```

    This is because of a concept called Automatic Semicolon Insertion, which automagically inserts semicolons after most newlines.
    In the first example, there is a semicolon inserted between the `return` statement and the object literal, so the function returns `undefined` and the object literal is never evaluated.

    * [**11.9.1** Rules of Automatic Semicolon Insertion](https://www.ecma-international.org/ecma-262/#sec-rules-of-automatic-semicolon-insertion)
    * [**13.10** The `return` Statement](https://www.ecma-international.org/ecma-262/#sec-return-statement)

## 使用数组访问对象属性

```js
var obj = { property: 1 }
var array = ['property']

obj[array] // -> 1
```

伪多维数组呢？

```js
var map = {}
var x = 1
var y = 2
var z = 3

map[[x, y, z]] = true
map[[x + 10, y, z]] = true

map["1,2,3"]  // -> true
map["11,2,3"] // -> true
```

??? note "💡 说明"

    The brackets `[]` operator converts the passed expression using `toString`.
    Converting a one-element array to a string is akin to converting the contained element to the string:

    ```js
    ['property'].toString() // -> 'property'
    ```

## 空和关系运算符

```js
null > 0;  // false
null == 0; // false

null >= 0; // true
```

??? note "💡 说明"

    Long story short, if `null` is less than `0` is `false`, then `null >= 0` is `true`.
    Read in-depth explanation for this [here](https://blog.campvanilla.com/javascript-the-curious-case-of-null-0-7b131644e274).

## `Number.toFixed()` 显示不同的数字

`Number.toFixed()` can behave a bit strange in different browsers.
Check out this 例:

```js
0.7875.toFixed(3)
    // Firefox: -> 0.787
    // Chrome: -> 0.787
    // IE11: -> 0.788
0.7876.toFixed(3)
    // Firefox: -> 0.788
    // Chrome: -> 0.788
    // IE11: -> 0.788
```

??? note "💡 说明"

    While your first instinct may be that IE11 is correct and Firefox/Chrome are wrong, the reality is that Firefox/Chrome are more directly obeying standards for numbers (IEEE-754 Floating Point), while IE11 is minutely disobeying them in (what is probably) an effort to give clearer results.

    You can see why this occurs with a few quick tests:

    ```js
    // Confirm the odd result of rounding a 5 down
    0.7875.toFixed(3) // -> 0.787
    // It looks like it's just a 5 when you expand to the
    // limits of 64-bit (double-precision) float accuracy
    0.7875.toFixed(14) // -> 0.78750000000000
    // But what if you go beyond the limit?
    0.7875.toFixed(20) // -> 0.78749999999999997780
    ```

    Floating point numbers are not stored as a list of decimal digits internally, but through a more complicated methodology that produces tiny inaccuracies that are usually rounded away by toString and similar calls, but are actually present internally.

    In this case, that "5" on the end was actually an extremely tiny fraction below a true 5.
    Rounding it at any reasonable length will render it as a 5...
    but it is actually not a 5 internally.

    IE11, however, will report the value input with only zeros appended to the end even in the toFixed(20) case, as it seems to be forcibly rounding the value to reduce the troubles from hardware limits.

    See for reference `NOTE 2` on the ECMA-262 definition for `toFixed`.

    * [**20.1.3.3** Number.prototype.toFixed (`fractionDigits`)](https://www.ecma-international.org/ecma-262//#sec-number.prototype.tofixed)

## `Math.max()` 少于 `Math.min()`

```js
Math.min(1,4,7,2)  // -> 1
Math.max(1,4,7,2) // -> 7
Math.min() // -> Infinity
Math.max() // -> -Infinity
Math.min() > Math.max() // -> true
```

??? note "💡 说明"

    * [Why is Math.max() less than Math.min()?](https://charlieharvey.org.uk/page/why_math_max_is_less_than_math_min) by Charlie Harvey

## 比较 `null` 和 `0`

The following expressions seem to introduce a contradiction:

```js
null == 0 // -> false
null >  0 // -> false
null >= 0 // -> true
```

How can `null` be neither equal to nor greater than `0`, if `null >= 0` is actually `true`? (This also works with less than in the same way.)

??? note "💡 说明"

    The way these three expressions are evaluated are all different and are responsible for producing this unexpected behavior.

    First, the abstract equality comparison `null == 0`.
    Normally, if this operator can't compare the values on either side properly, it converts both to numbers and compares the numbers.
    Then, you might expect the following behavior:

    ```js
    // This is not what happens
    null == 0
    +null == +0
    0 == 0
    true
    ```

    However, according to a close reading of the spec, the number conversion doesn't actually happen on a side that is `null` or `undefined`.
    Therefore, if you have `null` on one side of the equal sign, the other side must be `null` or `undefined` for the expression to return `true`.
    Since this is not the case, `false` is returned.

    Next, the relational comparison `null > 0`.
    The algorithm here, unlike that of the abstract equality operator, *will* convert `null` to a number.
    Therefore, we get this behavior:

    ```js
    null > 0
    +null = +0
    0 > 0
    false
    ```

    Finally, the relational comparison `null >= 0`.
    You could argue that this expression should be the result of `null > 0 || null == 0`; if this were the case, then the above results would mean that this would also be `false`.
    However, the `>=` operator in fact works in a very different way, which is basically to take the opposite of the `<` operator.
    Because our example with the greater than operator above also holds for the less than operator, that means this expression is actually evaluated like so:

    ```js
    null >= 0
    !(null < 0)
    !(+null < +0)
    !(0 < 0)
    !(false)
    true
    ```

    * [**7.2.12** Abstract Relational Comparison](https://www.ecma-international.org/ecma-262/#sec-abstract-relational-comparison)
    * [**7.2.13** Abstract Equality Comparison](https://www.ecma-international.org/ecma-262/#sec-abstract-equality-comparison)

## 同样的可变重新声明

JS允许重新声明变量：

```js
a;
a;
// This is also valid
a, a;
```

也在严格模式下工作：

```js
var a, a, a;
var a;
var a;
```

??? note "💡 说明"

    所有的辩护合并为一个定义。

    * [**13.3.2** 可变语句](https://www.ecma-international.org/ecma-262/#sec-variable-statement)