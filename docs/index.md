# WTFJS

## 什么是吊鸡丝

[![WTFPL 2.0][license-image]][license-url]
[![NPM version][npm-image]][npm-url]

> 有趣和棘手的JavaScript示例列表

JavaScript是一门伟大的语言。
它有一个简单的语法，庞大的生态系统，最重要的是一个伟大的社区。

与此同时，我们都知道JavaScript是一个非常有趣的语言，它有棘手的部分。
他们中的一些人可以迅速将我们的日常工作变成地狱，其中一些可以让我们大声笑出声来。

WTFJS最初的想法属于[Brian Leroux](https://twitter.com/brianleroux).
这份名单受到了他的演讲[** 2012年dotJS上的“WTFJS”**](https://www.youtube.com/watch?v=et8xNAc2ic8)的启发:

[![dotJS 2012 - Brian Leroux - WTFJS](https://img.youtube.com/vi/et8xNAc2ic8/0.jpg)](https://www.youtube.com/watch?v=et8xNAc2ic8)

## Node包装手稿

你可以使用`npm`来安装这本手册。
只要运行:

```bash
$ npm install -g wtfjs
```

你现在应该可以在命令行运行`wtfjs`。
这将在您选择的`$PAGER`中打开手册。
否则，你可以继续在这里阅读。

来源可在这里找到： <https://github.com/denysdovhan/wtfjs>

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

## 目录

- [💪🏻 Motivation](#-motivation)
- [✍🏻 Notation](#-notation)
- [👀 Examples](#-examples)
  - [`[]` is equal `![]`](#-is-equal-)
  - [true is false](#true-is-false)
  - [baNaNa](#banana)
  - [`NaN` is not a `NaN`](#nan-is-not-a-nan)
  - [It's a fail](#its-a-fail)
  - [`[]` is truthy, but not `true`](#-is-truthy-but-not-true)
  - [`null` is falsy, but not `false`](#null-is-falsy-but-not-false)
  - [`document.all` is an object, but it is undefined](#documentall-is-an-object-but-it-is-undefined)
  - [Minimal value is greater than zero](#minimal-value-is-greater-than-zero)
  - [function is not a function](#function-is-not-a-function)
  - [Adding arrays](#adding-arrays)
  - [Trailing commas in array](#trailing-commas-in-array)
  - [Array equality is a monster](#array-equality-is-a-monster)
  - [`undefined` and `Number`](#undefined-and-number)
  - [`parseInt` is a bad guy](#parseint-is-a-bad-guy)
  - [Math with `true` and `false`](#math-with-true-and-false)
  - [HTML comments are valid in JavaScript](#html-comments-are-valid-in-javascript)
  - [`NaN` is ~~not~~ a number](#nan-is-not-a-number)
  - [`[]` and `null` are objects](#-and-null-are-objects)
  - [Magically increasing numbers](#magically-increasing-numbers)
  - [Precision of `0.1 + 0.2`](#precision-of-01--02)
  - [Patching numbers](#patching-numbers)
  - [Comparison of three numbers](#comparison-of-three-numbers)
  - [Funny math](#funny-math)
  - [Addition of RegExps](#addition-of-regexps)
  - [Strings aren't instances of `String`](#strings-arent-instances-of-string)
  - [Calling functions with backticks](#calling-functions-with-backticks)
  - [Call call call](#call-call-call)
  - [A `constructor` property](#a-constructor-property)
  - [Object as a key of object's property](#object-as-a-key-of-objects-property)
  - [Accessing prototypes with `__proto__`](#accessing-prototypes-with-__proto__)
  - [``` `${{Object}}` ```](#-object-)
  - [Destructuring with default values](#destructuring-with-default-values)
  - [Dots and spreading](#dots-and-spreading)
  - [Labels](#labels)
  - [Nested labels](#nested-labels)
  - [Insidious `try..catch`](#insidious-trycatch)
  - [Is this multiple inheritance?](#is-this-multiple-inheritance)
  - [A generator which yields itself](#a-generator-which-yields-itself)
  - [A class of class](#a-class-of-class)
  - [Non-coercible objects](#non-coercible-objects)
  - [Tricky arrow functions](#tricky-arrow-functions)
  - [`arguments` and arrow functions](#arguments-and-arrow-functions)
  - [Tricky return](#tricky-return)
  - [Accessing object properties with arrays](#accessing-object-properties-with-arrays)
  - [`Math.max()` less than `Math.min()`](#mathmax-less-than-mathmin)
  - [Null and Relational Operators](#null-and-relational-operators)
  - [`Number.toFixed()` display different numbers](#numbertofixed-display-different-numbers)
  - [Comparing `null` to `0`](#comparing-null-to-0)
  - [Same variable redeclaration](#same-variable-redeclaration)
- [Other resources](#other-resources)
- [🎓 License](#-license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 💪🏻 动机

> 只是为了好玩
>
> &mdash; _[**“只是为了好玩: 一个意外革命家的故事”**](https://en.wikipedia.org/wiki/Just_for_Fun), Linus Torvalds_

如果可能的话，这个清单的主要目标是收集一些疯狂的例子并解释它们是如何工作的。
只是因为学习之前我们不知道的东西很有趣。

如果您是初学者，可以使用这些笔记深入了解JavaScript。
我希望这些笔记能激励你花更多的时间阅读规范。

如果您是专业开发人员， 您可以将这些示例作为我们心爱的JavaScript的所有怪癖和意想不到的边缘的一个很好的参考。

无论如何，只要阅读这个。
你可能会找到新的东西。

## ✍🏻 符号

**`// ->`** 用于显示表达式的结果。

例如:

```js
1 + 1 // -> 2
```

**`// >`** 意味着`console.log`或其他输出的结果。

例如:

```js
console.log('hello, world!') // > hello, world!
```

**`//`** 只是用于解释的评论

例:

```js
// Assigning a function to foo constant
const foo = function () {}
```

## 其他资源

- [wtfjs.com](http://wtfjs.com/) — 一系列非常特殊的违规行为，不一致以及网络语言的简单痛苦不直观的时刻。
- [Wat](https://www.destroyallsoftware.com/talks/wat) — 来自CodeMash 2012的Gary Bernhardt闪电谈话
- [What the...JavaScript?](https://www.youtube.com/watch?v=2pL28CcEijU) — Kyle Simpsons谈论Forward 2尝试从JavaScript中“抽出疯狂”。他希望帮助您生成更干净，更优雅，更易读的代码，然后激励人们为开源社区做出贡献。

## 🎓 证书

[![CC 4.0][license-image]][license-url]

&copy; [Denys Dovhan](http://denysdovhan.com)

[license-url]: http://www.wtfpl.net
[license-image]: https://img.shields.io/badge/License-WTFPL%202.0-lightgrey.svg?style=flat-square

[npm-url]: https://npmjs.org/package/wtfjs
[npm-image]: https://img.shields.io/npm/v/wtfjs.svg?style=flat-square
