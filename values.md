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
* Objects are immutable which makes them easier to reason about and share amongst threads.
* There is no hidden state that affects the result of a function.
* There are no side-effects to calling a function.
* They are easy to unit test.

The last benefit, that they are easy to unit test, is important. Without hidden state (eg, a network connection, a test DB, a tmp dir, etc) the "functional objects" are easy to setup and run tests against them. There is little need for mocks/stubs because plain old objects can be used.

In Gary's talk he says they have lots of behavior but few dependencies.

## Imperative Shell

If the functional objects in the core are immutable then where do the results of calling functions on them go? They bubble up to an imperative, stateful, shell of code. This shell allows variables to be overwritten, algorithms to execute step by step (and perhaps fail), and also reads bytes from the outside world and writes bytes to the outside world.

Unfortunately, this shell of code is hard to test because it deals with external services and IO. For example, to test some part of its behavior a real connection to a DB could be required. There are obvious drawbacks to that so a mock of some kind could be used, but then the risk is that the tests pass but the application fails in production.

## To Summarize So Far

As Gary says in his talk, the functional core has lots of behavior (code paths), but few dependencies. Whereas, the imperative shell has little behavior (code paths), but lots of dependencies. Integration tests can be used to ensure the application wires-up correctly and unit tests can be used to ensure all the behavior is correct.


That makes it ideal for integration tests. The 

## Values

Honestly, I've found it tricky to come up with my own definition of what a value is. But I think this works:
1. Values are immutable.
1. Values are serializable.
