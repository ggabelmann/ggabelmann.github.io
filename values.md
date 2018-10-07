# Functional Objects and Shifting Boundaries

Gary Bernhardt's [Boundaries](https://www.destroyallsoftware.com/talks/boundaries) talk is rightfully famous. But what does he mean when he 
says that values allow boundaries to shift?

## Functional Programming

If we have the function: `C = f(A, B)`

Then functional programming means that the result `C` depends only on the inputs `A` and `B`. If `A` and `B` are unchanging then the result will always be `C`, no matter the state of the rest of the system. Furthermore, there will be no side-effect `D` to calling the function; only the result `C`.

To make things more object-oriented we can instead write: `C = A.f(B)`

Again, if things are unchanging then the result will always be `C`. `A` and `B` won't be changed either because that would be a side-effect.

To give an example in Java we can use a String:
```java
final String s = "hello";
final String r = s.concat(" world"); // s is unchanged
```

I call that a "functional object", but another name for it could be "value". [1]

## Functional Core

Having a core of functional objects has many benefits:
* Objects are immutable which makes them easier to reason about and share amongst threads.
* There is no hidden state that affects the result of a function.
* There are no side-effects to calling a function.
* They are easy to unit test.

The last benefit, that they are easy to unit test, is important. Without hidden state (eg, a network connection, a test DB, a tmp dir, etc) the functional objects are easy to setup and run tests against them. There is little need for mocks/stubs because plain old objects can be used.

## Imperative Shell

If the functional objects in the core are immutable then where do the results of calling functions on them go? The results bubble up to an imperative, stateful, shell of code. This shell may have global state, can overwrite variables, has algorithms that execute step by step (and perhaps fail), and also reads bytes from the outside world and writes bytes to the outside world.

Unfortunately, this shell of code is hard to test because it deals with external services and IO. For example, to test some part of its behavior a real connection to a DB could be required. There are obvious drawbacks to that so a mock of some kind could be used, but then the risk is that the tests pass but the application fails in production (because the mock was an imperfect representation of the DB).

## To Summarize So Far

As Gary says in his talk, the functional core has lots of behavior (code paths), but few dependencies. Whereas, the imperative shell has little behavior (code paths), but lots of dependencies. Integration tests can be used to ensure the application wires-up correctly and unit tests can be used to ensure all the behavior is correct. Moving code out of the imperative shell and into the functional core is an important strategy to ensuring the application can be tested well.

## Boundaries

The imperative shell acts as a boundary between the functional core and the outside world of things that change and fail. In its most basic form, communication over that boundary occurs via bytes: serialized values (a primitive or an object without methods), or to use the language of this post, serialized functional objects.

For example, if the application is waiting for the user to press a key then the result will likely be an integer. That could range from an `int` (a value/primitive), to an `Integer` (a simple functional object), or even to a `KeyPress` (a more complex functional object).

The boundary between the application and the outside world therefor shifts depending on which functional objects are used. For example:
* A distributed system that is able to read/write high-level values (ie, functional objects) vs. one that can't share information. [2]
* An image editor that uses a robust image format for its work vs. one that is only able to read/write bitmaps.
* A container system (eg, Docker) that uses a configuration file with many options vs. a system that can only read TAR files.

## Conclusion

Using functional objects in an application has some major benefits:
* The core of functional objects is easily tested.
* They allow the application to communicate more information with the outside world.
* The remaining imperative code naturally collects into a layer that separates the application's core from the outside world.

## Footnotes

[1] One definition of a value is: a thing which can't be tested for equality by testing pointers/references, instead its value has to be tested. A functional object can then be called a value if it's composed of a graph of values. Whereas if it has references to services/etc then those can't be converted to values and tested, so it's not a value.

[2] A useful feature for a distributed system is the ability to share high-level information between its nodes. But why can't non-functional objects be shared? I'm not saying they can't be, just that it's probably harder to de/serialize them. If an object has a whole graph of references inside it that refer to services, streams of IO, etc, then how can it be shared with other distributed nodes? A functional object on the other hand is basically just a graph of primitives, something that is easy to de/serialize.
