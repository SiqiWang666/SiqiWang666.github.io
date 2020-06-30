---
title: "Dynamic Typing❓Static Typing❓"
date: 2020-06-28
read_time: false
categories:
  - Blog
tags:
  - C#
---

## What is Dynamic Typing?

I always see the sentences like "Some language, like, python or javascript, supports _dynamic typing_". You can assign a value to a variable and later assign a different type of value to it. I also heard the term _untyped_ language. This is kind of ambiguous for me.

```javascript
// javascript example
var i = 3;
console.log(typeof i);
```

What's the output of above code? `number` right? So, it is not truely untyped, but `untyped = dynamic typing`. The type of a variable is determined during runtime. _Untyped language_ doesn't have static type. It is pretty straightforward because javascript is not a compiled language. So it doesn't have type checking during compile time like typed languages do.

## Static Typing?

For compiled languages, like C# or Java, specifying the type of the value (static type) when you declare it is a must. Otherwise, it will throw compile error.

```c#
// C# example
int a = 3;
a = "Hello World!"; // Compilation Error
```

### Question: What's the output of following code snippet❓

Class C inherits from class B and class B inherits from class A.

```c#
// C# example
A a = new C();
B b = a; // Compilation Error
```

Because the static type reference of `a` is `A` and the static type reference of `b` is `B`.

> Solution. Casting `a`, like `B b = (B) a`

Follow up question❓

```c#
// C# example
Console.WriteLine(a is C); // True
Console.WriteLine(a is B); // True
```

## `var` and `dynamic`

Both `var` and `dynamic` are the keywords that are used to modify a variable for dynamic typing in C#.

```c#
var a = new int[]{1,2,3};
dynamic b = "Hello World!";
```

### `var` vs `dynamic`

1. `var` figures out the type during _compiled time_. The variable must be declared with initialization.
2. `dynamic` figures out the type during _tuntime_. Value can be initialized later after declaration.
