# What is Functional Programming

FP can be defined in two statements:<br>
1. FP is about writing software applications using only pure functions.
2. When writing FP code you only use immutable values - [why?][here]

## Pure Function
This can be defined as:
1. The o/p of the function depends only its input and internal algorithm unlike in OOP method which can depend on instance/class or method variable.
2. has no side effects. Does not read from a file, web service, UI or DB and does not write either.

- Recursion is a by-product of FP and not a requirement. So, if you only use pure functions and immutable values, the *only* way you can do things like sum of a list is by using recursion.
- A good reason to use only immutable values is that mutable variables don’t work well with parallel/concurrent applications. Because concurrency is becoming more important as CPUs use more cores.

# Benefits of Functional Programming

1. Pure functions are easier to reason about
	- pure functions are easier to reason about because you know that they can’t do certain things, such as talk to the outside world, have hidden inputs, or modify hidden state. Because of this, you’re guaranteed that their function signatures tell you (a) exactly what’s going into each function, and (b) coming out of each function.
	- easier to read and understand.
2. Another way that pure functions make code easier to reason about won’t be apparent when you’re first getting started. It turns out that what really happens in FP applications is that (a) you write as much of the code as you can in a functional style, and then (b) you have other functions that reach out and interact with files, databases, web services, UIs, and so on — everything in the outside world.
3. The concept is that you have a “Pure Function” core, surrounded by impure functions that interact with the outside world:<br>
![pureImage][pureImage]<br>
it provides a clean separation between pure and impure functions — so clean that you can tell by looking at a function’s signature whether it is pure or impure.

4. Testing is easier- you don’t have to worry about them dealing with hidden state and side effects. Impure functions can change states of outside world variables and thus can make unit-testing tough! In case of pure functions, developer doesn't have to worry about that bit.
5. Debugging is easier
6. Parallel/Concurrent programming is easier: A functional program is ready for concurrency without any further modifications. You never have to worry about deadlocks and race conditions because you don’t need to use locks. No piece of data in a functional program is modified twice by the same thread, let alone by two different threads. That means you can easily add threads without ever giving conventional problems that plague concurrency applications a second thought.<br>
![imgConc][imgConc]
> *Did you notice that in the definition of a data race there’s always talk of mutation? Any number of threads may read a memory location without synchronization, but if even one of them mutates it, you have a race. And that is the downfall of imperative programming: Imperative programs will always be vulnerable to data races because they contain mutable variables.*



[here]: <https://www.quora.com/Why-is-immutability-important-in-functional-programming>
[pureImage]: <https://github.com/penguinmishra/images_repo/blob/master/Functional%20Programming%20Basics/pure-fp-core-v2.png>
[imgConc]: <https://github.com/penguinmishra/images_repo/blob/master/Functional%20Programming%20Basics/fp-concurrency-story.jpg>