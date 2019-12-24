# Functional Programming Basics

contains following concepts:
- Functions as first class objects
- Pure functions
	- No state
	- No side effects
	- Immutable variables
	- Favour recursion over looping
- High Order functions

## First class citizens : first class objects

There are no restrictions on the object's use. A first class object is an entity that can be dynamically created, destroyed, passed to a function, returned as value and have all the rights as other variables in the programming language.
The closest java methods get to first class objects are lambda expressions.

## Pure Functions
If
- execution of function has no side effects
- the return value of the function depends only on the input parameters passed to the function. example:
```
public class ObjectWithPureFunction{

    public int sum(int a, int b) {
        return a + b;
    }
}
```
Here is an example of non-pure function:
```
public class ObjectWithNonPureFunction{
    private int value = 0;

    public int add(int nextValue) {
        this.value += nextValue;
        return this.value;
    }
}
```
## High Order Functions
A function is high order function if at least one of the following conditions are met:
- function takes one or more functions as parameters
- function returns another function as result.
- In Java, the closest we can get to a higher order function is a function (method) that takes one or more lambda expressions as parameters, and returns another lambda expression. Here is an example:
```
public class HigherOrderFunctionClass {

    public <T> IFactory<T> createFactory(IProducer<T> producer, IConfigurator<T> configurator) {
        return () -> {
           T instance = producer.produce();
           configurator.configure(instance);
           return instance;
        }
    }
}
```

## No State
- no external state of the function.
- A function may have local variables containing temporary state internally but the function cannot reference any member variables of the class or object the function belongs to.
```
public class Calculator {
    public int sum(int a, int b) {
       return a + b;
    }
}
```
Contrarily, the below function uses external state:
```
public class Calculator {
    private int initVal = 5;
    public int sum(int a) {
       return initVal + a;
    }
}
```

## No Side Effects

- function cannot change any state outside of the function.
- changing state outside of the function is referred to as *side effect*.

## Immutable Variables

## Recursion over looping
 - recursion uses function calls to achieve looping and so the code becomes more functional. Streams API is another alternate to loops whcih is functionally inspired.


# Pure functions and IO
How can an FP application possibly get anything done if all of its functions are pure functions because pure functions cannot have IO?<br>
> *“How do I get anything done if I can’t read any inputs or write any outputs?”*<br>
Answer is to violate 'Write only Pure Function' principal.<br><br>
Wrapping an input from outside world in an IO does not make this a pure function but is impure still: `IO[String]`<br><br>
when you need to write I/O code in functional programming languages, the solution is to violate the “Only Write Pure Functions” rule. The general idea is that you write as much of your application as possible in an FP style, and then handle the UI, Database I/O, Web Service I/O, and File I/O in the best way possible for your current programming language and tools.