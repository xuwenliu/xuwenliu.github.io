---
title: TS中的枚举
index_img: /img/ts/ts.jpg
tags: [TS]
category: 技术
abbrlink: f94fb1be
date: 2020-05-06 22:14:00
---

### 1.枚举

> 使用枚举我们可以定义一些带名字的常量。 使用枚举可以清晰地表达意图或创建一组有区别的用例。 TypeScript支持数字的和基于字符串的枚举。

### 2.数字枚举

```ts
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
```
定义了一个数字枚举， `Up`使用初始化为 `1`。 其余的成员会从 `1`开始自动增长。 换句话说， `Direction.Up`的值为 `1`， `Down`为 `2`， `Left`为 `3`， `Right`为 `4`。

不使用初始化器`Up`的值为 `0`， `Down`的值为`1`

### 3.字符串枚举

> 字符串枚举的概念很简单，但是有细微的 [运行时的差别](https://www.tslang.cn/docs/handbook/enums.html#enums-at-runtime)。 在一个字符串枚举里，每个成员都必须用`字符串字面量`，或另外一个字符串枚举成员进行初始化。

```ts
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}
```

字符串枚举没有自增长的行为

### 4.异构枚举

```ts
enum BooleanLikeHeterogeneousEnum {
    No = 0,
    Yes = "YES",
}
```

从技术的角度来说，枚举可以混合字符串和数字成员，但并不推荐这么做。

### 5.计算的和常量成员

> 每个枚举成员都带有一个值，它可以是 `常量`或 `计算`出来的。 当满足如下条件时，枚举成员被当作是常量：
+ 它是枚举的第一个成员且没有初始化器，这种情况下它被赋予值 `0`

  ```ts
  // Direction.Up is constant: 0
  enum Direction { Up }
  ```

+ 它不带有初始化器且它之前的枚举成员是一个 *数字*常量。 这种情况下，当前枚举成员的值为它上一个枚举成员的值加1。

  ```ts
  enum Direction {
      Up = 1,
      Down,
      Left,
      Right
  }
  ```

+ 枚举成员使用 `常量枚举表达式`初始化。 常量枚举表达式是TypeScript表达式的子集，它可以在编译阶段求值。 当一个表达式满足下面条件之一时，它就是一个常量枚举表达式：

  + 一个枚举表达式字面量（主要是字符串字面量或数字字面量）
  + 一个对之前定义的常量枚举成员的引用（可以是在不同的枚举类型中定义的）
  + 带括号的常量枚举表达式
  + 一元运算符 `+`, `-`, `~`其中之一应用在了常量枚举表达式
  + 常量枚举表达式做为二元运算符 `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `>>>`, `&`, `|`, `^`的操作对象。 若常数枚举表达式求值后为 `NaN`或 `Infinity`，则会在编译阶段报错。

  ```ts
  enum FileAccess {
      // constant members
      None,
      Read    = 1 << 1,
      Write   = 1 << 2,
      ReadWrite  = Read | Write,
      // computed member
      G = "123".length
  }
  ```

### 6.反向映射

  > 除了创建一个以属性名做为对象成员的对象之外，数字枚举成员还具有了 *反向映射*，从枚举值到枚举名字。 例如，在下面的例子中：

  ```TS
  enum Enum {
      A
  }
  let a = Enum.A;
  let nameOfA = Enum[a]; // "A"
  ```

  要注意的是*不会*为字符串枚举成员生成反向映射。