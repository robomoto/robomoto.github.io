---
layout: post
title:  "Python Object Mutability Caveats"
date:   2025-06-29 21:38:00 -0700
categories: python
---

 I have been working through [Dr. Fred Baptiste's Python 3 Deep Dive](https://www.udemy.com/course/python-3-deep-dive-part-1/) 

I'm writing about some of the things I find that I had not intuited that I find there. I find doing so helps set things in my mind, and if that fails, it also gives me something to refer back to. 

## Object Mutability

I have vague memories from school, where a professor would mention that a particular data type is mutable or immutable, but I don't recall them ever talking about when you might prefer one over the other in any deeper sense than *"use a mutable data type if you need to be able to modify it."* On the surface, that seems like a reasonable rationale. But a more in-depth understanding is helpful.

**Immutable Objects**  
: Objects whose elements cannot be inserted, replaced, or deleted.

**Mutable Objects**  
: Objects whose elements can be inserted, replaced, or deleted.

| Immutable Objects         | Mutable Objects |
|---------------------------|-----------------|
| Numbers (int, float, etc) | Lists           |
| Strings                   | Sets            |
| Tuples                    | Dictionaries    |
| Frozen Sets               |                 |

User-defined classes can fall into either category.

One interesting caveat that I had never considered is that you can have immutable objects that reference mutable data types. In that case, the memory references in the immutable object won't change, but the contents at that location in memory may change.

For example, consider this code:

```python
a = [1, 2]
b = ["some", "list"]
t = (a, b)
t
```

In the above, we have created a tuple (t) that holds two lists.
```text 
([1, 2], ['some', 'list'])
```
 We can't change t, but we can change a and b.  

```python
a.append(3)
b.insert(0, "Hi!")
t
```
Tada!
```
([1, 2, 3], ['Hi!', 'some', 'list'])
```

The practical lesson here is to be careful when using an immutable datatype.  If it contains references to mutable data, you can't guarantee that the information you're reading won't change.  There may be times you want that, but you need to think about it. 

