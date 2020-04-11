# Language Stuff #

## Typeclasses ##

Before learning to implement [typeclasses](https://en.wikipedia.org/wiki/Type_class), you should first be at least someone familiar with what they are and how they work.
[Swift](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html) has typeclass support, specifically with [protocols](https://docs.swift.org/swift-book/LanguageGuide/Protocols.html) (which introduce a new typeclass) and [extensions](https://docs.swift.org/swift-book/LanguageGuide/Extensions.html) (which implement an existing typeclass for an existing type).
Other languages have typeclasses too (e.g., `class` and `instance` in [Haskell](https://www.haskell.org/), `trait` and `impl` in [Rust](https://www.rust-lang.org/)), though Swift will probably be the quickest path to understanding them.

Wadler and Blott introduced typeclasses in &ldquo;How to Make Ad-hoc Polymorphism Less Ad-hoc&rdquo;, which is available [here](https://people.csail.mit.edu/dnj/teaching/6898/papers/wadler88.pdf).
These were originally introduced in Haskell, and the paper is phrased in terms of a translation from Haskell with typeclasses to Haskell without typeclasses.
This is the definitive resource for understanding how to implement typeclasses, though the paper requires some knowledge of Haskell.

Michael, Liad, and Shant implemented support for typeclasses in [Funkadelic](https://github.com/csun-comp430-s19/funkadelic), which was developed for my Spring'19 offering of COMP 430.
Their implementation was in Haskell (no escape from Haskell!).


## Overloading ##

The basic idea behind [function/method overloading](https://en.wikipedia.org/wiki/Function_overloading) is that we use both the function/method name, as well as the types and number of the function/method parameters, to determine exactly what function/method to call.
This can be resolved entirely at compile time.
For example, consider the following C-like code:

```c
int foo(int x, int y) { return x + y; }
int foo(int z) { return z; }
// ...
int callsFirst = foo(1, 2);
int callsSecond = foo(3);
```

Looking at the parameters involved, we can automatically translate the above C-like code to an equivalent which does not use overloading, shown below:

```c
int foo_int_int(int x, int y) { return x + y; }
int foo_int(int z) { return z; }
// ...
int callsFirst = foo_int_int(1, 2);
int callsSecond = foo_int(z);
```

Notably, at compile time, we know exactly which method needs to be called.
It's just that the resolution process of determining which method this is now needs to consider both the name and parameter types involved.

I unfortunately don't have any examples handy which show this exactly.
The closest thing I can show is [this code](https://github.com/csun-comp430-s19/vtables-example/blob/d54727fccbe488b81bce6546ea0740a746e07673/src/main/java/vtables_example/typechecker/Typechecker.java#L122), which is taken from the typechecker of an example COMP 430 compiler.
This specific line is used to determine exactly which method is called.
Currently, the check only looks at the name of the method, as this language doesn't support overloading.
However, with overloading, this check would have to consider both the method name and types.
