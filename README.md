# Introduction

Optimizing recursion through techniques like *Tail Call Optimization*, *Continuations Passing Style* and *Trampolines* can allow recursive functions to run in constant O(1) time, rather than the O(n) time of normal recursion. In this article, we'll explore these optimization approaches using a factorial function as an example running throughout the article.

# Normal Recursion: Problems and Complexity

Normal recursive functions use a stack to keep track of function calls. This has **O(n)** linear time complexity where **n** is the recursion depth. For example:

```javascript
function factorial(n) {
   if (n <= 1) return 1;  
   return n * factorial(n-1);
}
```

Each call pushes a new stack frame, using more memory and time. For large inputs, this can cause stack overflows.

To overcome these problems, we need to optimize our recursive functions using techniques like Tail Call Optimization, Continuation Passing Style, and Trampolines. These approaches provide efficient ways to execute recursive functions without consuming excessive memory or causing stack overflow errors.

But before exploring the three techniques mentioned above, first, we have to understand Proper Tail Call (PTC).

# Proper Tail Call (PTC)

A proper tail call is a **recursive function call** that is the **last expression** evaluated in the function. A typical example:

```javascript
function increment(x) {
    return increment(x+1)
}
```

It also needs to have the following characteristics:

* The recursive call is the last line of code executed in the function. Nothing is done after the call returns.
    
* The function returns the result of the recursive call directly, without modifying it.
    
* No closure variables are accessed after the recursive call. All variable accesses happen before the call.
    
* The recursive call passes all necessary arguments, so no additional logic is needed after the call returns.
    

Let's refactor the factorial code into a Proper tail call following these characteristics:

```javascript
function factorial(n) {
  if (n <== 1) return 1;  
  return n * factorial(n - 1);
}
```

Here, we are satisfying the first characteristics i.e. the recursive call should be the last line of code. But failing the second characteristic as we are modifying the recursive call during return.

We can fix this by passing the accumulated value as an argument to the next recursive call.

```javascript
function factorial(n, acc=1) {
  if (n <== 1) return acc;  
  return factorial(n - 1, n * acc);
}
```

And now we are also satisfying other characteristics as well.

But, what's the benefit of doing this? 🤔

If we transform any recursive function call into a proper tail call then the compilers for that language will understand that this should be executed in O(1) space complexity applying the Tail Call Optimization technique, instead of O(n) space complexity. The reason behind this space optimization is due to the way these proper tail call recursive functions are going to be executed.

How a Proper Tail Call recursive function is executed by using the Tail Call Optimization technique?

# Tail Call Optimization (TCO)

Tail Call Optimization is a technique used by some programming languages to optimize tail recursive function calls.

Every language has its compiler and has its different rules of execution. So, let's pick Chrome's javascript compiler and try to understand what is happening behind the scenes.

As soon as the compiler sees the Proper Tail Call it changes its normal execution behavior of recursive function execution i.e. stacking each new recursive call on top of the call stack, instead, it removes the recursive call which is executed and reuses the stack for the next recursive call.

This way we only have one recursive call at a time, optimizing memory with a space complexity of O(1).

Several major programming languages support tail call optimization, including:

* Functional languages:
    
    * Lisp - One of the earliest languages to support TCO
        
    * Scheme
        
    * OCaml
        
    * Haskell
        
* Recursion-friendly languages:
    
    * Python (since 3.5)
        
    * Ruby (since 2.1)
        
    * Java (since Java 8)
        
    * C# (.NET 4.0)
        
* JavaScript (since ES2015)
    
* Go
    

Some other notable languages that support tail call optimization are:

* Clojure
    
* Scala
    
* Erlang
    
* Elixir
    
* Rust
    
* Kotlin
    
* Swift
    

Not all languages support tail call optimization though. Some that don't include:

* C
    
* C++
    
* PHP
    
* Java (before Java 8)
    

What to do if a certain language does not support Tail Call Optimization, we can't rely only the compilers, right?

Other techniques could be used to achieve Tail Call Optimization benefits:

1. Continuation passing style
    
2. Trampolines
