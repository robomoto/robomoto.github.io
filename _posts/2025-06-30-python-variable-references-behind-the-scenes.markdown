---
layout: post
title:  "Python Variable References Behind the Scenes"
date:   2025-06-29 21:38:00 -0700
categories: python
---

## Python Variable References

As much as I have learned about how to use Python structures, I hadn't ever gone too in depth on why it works the way it does, or the logical extensions of Python's development approach until recently. 

### Shared References and Interning in CPython

CPython will automatically intern certain immutable data values.  For example, the integers from -5 to 256.  When you declare variables with the same integer value in that range, they will reference the same address in memory:

```py
x = 25
y = 25

print(hex(id(x)), hex(id(y)))
print(x == y, x is y)
```

```sh
0x1056876c8 0x1056876c8
True True
```

In the above code snippet, I'm using the `is` identity operator, which is how you tell if two objects reference the same address in memory.  

### NoneType is immutable and interned

One little nicety of Python interning commonly used values is that all references to `None` reference the same memory address within your running application.  

```py
a = None
print(hex(id(a)))

b = None
print(hex(id(b)))

a is b
```
```sh
0x105662720
0x105662720
True
```

In practice, this just means that anytime you're trying to tell if a variable has a non-null value, you can use the `is` identity operator which reads nicely.

### Fancy usage of `and` and `or`

Consider this:
```py
a = None

if a:
  print(a)
else:
  print("'a' has no value")
```

Python's `or` operator works such that if a is truthy, it returns a, and if it's falsy, it evaluates and returns the second half of the expression, so this is equivalent to the above statement.

```py
print(a or "'a' has no value")
```

I still can't decide if that's really clever, cool, and useful; or irritating, irregular, and confusing.  Probably both.  I guess all I can do is try to put it in a PR and see what kind of comments it gets. 
