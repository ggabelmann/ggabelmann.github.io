## Work In Progress

Gary Bernhardt's [Boundaries](https://www.destroyallsoftware.com/talks/boundaries) talk is rightfully famous. But what does he mean when he 
says that values allow boundaries to shift?

## Functional Programming

If we have the function: **C = f(A, B)**

Then functional programming means that the result **C** depends only on the inputs **A** and **B**. If **A** and **B** are unchanging then the result will always be **C**, no matter the state of the rest of the system. Furthermore, there will be no side-effect **D** to calling the function; only the result **C**.

To make things more object-oriented we can instead write: **C = A.f(B)**

Again, if things are unchanging then the result will always be **C**. **A** and **B** won't be changed either because that would be a side-effect.

To give an example in Java we can use a String:
```java
final String s = "hello";
final String r = s.concat(" world"); // s is unchanged
```

## Functional Core

Having a core of "functional objects" has a lot of benefits:
* Objects are immutable which makes them: easier to reason about, and share amongst threads.
* There is no hidden state that affects the result of a function.
* There are no side-effects to calling a function.
* They are easy to unit test.

## Values

Honestly, I've found it tricky to come up with my own definition of what a value is. But I think this works:
1. Values are immutable.
1. Values are serializable.
