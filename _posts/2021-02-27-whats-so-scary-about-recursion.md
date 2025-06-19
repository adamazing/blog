---
title: "What's so scary about recursion?"
date: 2021-02-27T17:00:00-12:00
categories:
  - blog
tags:
  - programming
  - recursion
  - maths
header:
    image: /assets/images/posts/2021-02-27-whats-so-scary-about-recursion/1_OALoRc9L3dKBhXsDLxi_JQ.webp
    caption: "The B in Benoit B. Mandelbrot stands for Benoit B. Mandelbrot"
---


The [Fibonacci sequence](https://oeis.org/A000045) is one of the better known sequences of numbers, and one of my favourites. If you’ve never seen it — or, more likely, if you only ever saw it in some long-forgotten maths lesson and need a refresher — here are its first 10 terms:

> 0, 1, 1, 2, 3, 5, 8, 13, 21, 34

The Fibonacci sequence is defined by the following, __*recursive*__, function:

> f(n) = f(n-1)+f(n-2)

Starting from:

> f(0) = 0 & f(1) = 1

That is, after the first two terms, each successive term is the sum of the two terms before it.

If it’s clearly a recursive function, why would you use iteration to find the nth term of the Fibonacci sequence? Why reach for your trusty __*for*__ loop?

Is it because recursion is hard? Let’s have a look at an iterative solution using a simple for loop in Javascript:

```javascript
const fibonacci = n => {
  if(n<=1) return n       //f(0)=0, f(1)=1
  let [a,b,c] = [0,1,1]   
  for(let i = 1;i<n;i++){
    c = a + b
    a = b
    b = c
  }
  return c
}
```

So far so good. If we run this to find the 50th term of the Fibonacci sequence:

```
$ node ./fibonacci-iterative.js
12586269025
Calculated in 2ms
```

2 milliseconds! [Pretty, pretty, pretty good](https://www.youtube.com/watch?v=O_05qJTeNNI). Now let’s look at a recursive solution:

```javascript
const fibonacci = n => {
  if(n<=1) return n                       // f(0)=0, f(1)=1
  return fibonacci(n-1) + fibonacci(n-2)  // f(n) = f(n-1) + f(n-2) 
}
```

[So recursion. Many elegance!](https://en.wikipedia.org/wiki/Doge_(meme)) The key tenet of recursion is to gradually reduce the problem until we get to the simplest possible solution. 

When we start to write our recursive function, we start with our “base case”. In this case it’s the values to return for the first two terms. For greater values of n we simply return the sum of the previous two numbers in the sequence, which we calculate using the function itself. 

See how similar it is to the mathematical definition? If anything, the recursive solution is easier to reason about than having to shuffle variables around, and risking an off-by-one error in defining our for-loop!

Let’s run this bad-boy to find the 50th term of the Fibonacci series and see it in action!

```
$ node ./fibonacci-recursive.js
12586269025
Calculated in 148937ms
```

Ok…yikes 😬. More than 2 minutes? Our beautiful solution betrayed us! What happened? Take a minute to think about why that might be…

![Photo by Brad Neathery on Unsplash](/assets/images/posts/2021-02-27-whats-so-scary-about-recursion/1_C_XHSkv4cxBTEIl5_h_Bpg.webp)

Let’s think about what’s happening when we call the recursive function with a specific value:

> f(5) = f(4) + f(3)

So the function returns the result of: __*f(4) + f(3)*__. But what’s happening when we call the function for __*n=4*__?:

> f(4) = f(3) + f(2)

Huh. So our recursive approach is calculating the value of __*f(3)*__ twice, once when calculating the value of __*f(5)*__ and once when calculating the value of __*f(4)*__. That’s…not ideal. [But wait, there’s more!](https://tvtropes.org/pmwiki/pmwiki.php/Main/ButWaitTheresMore) Consider what happens when we call __*f(3)*__:

> f(3) = f(2) + f(1)

[Crumbs!](https://en.wikipedia.org/wiki/Danger_Mouse_(1981_TV_series)) You know what that means, don’t you? If __*f(4)*__ is calling __*f(2)*__ once and __*f(3)*__ (which is called twice) is also calling __*f(2)*__, that means __*f(2)*__ is being called a total of __*3 times*__.

You can see this by running the recursive solution with some logging:

```javascript 
const fibonacci = n => {
  if(n<=1) return n
  console.log(`f(${n}) = f(${n-1}) + f(${n-2})`)
  return fibonacci(n-1) + fibonacci(n-2)
}
```

Which, for __*n=5*__ will log:

```
f(5) = f(4) + f(3)
f(4) = f(3) + f(2)
f(3) = f(2) + f(1)
f(2) = f(1) + f(0)
f(2) = f(1) + f(0)
f(3) = f(2) + f(1)
f(2) = f(1) + f(0)
```

Can you see the pattern? Take a moment. What about if I add some more data-points by running it for __*n=10*__, collate the calls, and tabulate that with [console.table()](https://developer.mozilla.org/en-US/docs/Web/API/Console/table)?:

```
┌─────────┬────────┐
│ (index) │ Values │
├─────────┼────────┤
│  f(10)  │   1    │ 
│  f(9)   │   1    │
│  f(8)   │   2    │
│  f(7)   │   3    │
│  f(6)   │   5    │
│  f(5)   │   8    │
│  f(4)   │   13   │
│  f(3)   │   21   │
│  f(2)   │   34   │
│  f(1)   │   55   │
│  f(0)   │   34   │  
└─────────┴────────┘
```

Now **there’s** ya recursion! If you’d worked it out already, give yourself a pat on the back. If you had worked it out already __and__ you kept reading before jumping into the comments to berate me for that naive implementation, you get a gold star.

It’s perhaps obvious, in hindsight, why this would be so. 

![Animation illustrating the branching function calls](/assets/images/posts/2021-02-27-whats-so-scary-about-recursion/recursive-function-calls.gif)

It should hopefully be obvious why our naive recursive solution will have problems generating Fibonacci numbers much larger than the 50th too; when calculating __*f(50)*__, __*f(1)*__ will be called __12,586,269,025__ times. As __*n*__ becomes large, f(n) grows exponentially. With n much larger than 50, the stack will overflow.

![A sad looking pug wrapped in a blanket. Credit: Matthew Henry on Unsplash](/assets/images/posts/2021-02-27-whats-so-scary-about-recursion/0_4sg6XAkTWXqWqJwK.webp)

What can we do? We could [memoize](https://en.wikipedia.org/wiki/Memoization) the fibonacci function so that it never calculates the same number twice:

```javascript
const memo = {}
const fibonacci = n => {
 if(n<=1n) return n
 if(!memo[n])
   memo[n] = fibonacci(n-1n) + fibonacci(n-2n)
 return memo[n]
}
```

By doing that — caching the calculated values — we can calculate the __*100th*__ Fibonacci number in almost no time at all:


```
$ node ./fibonacci-memoised-recursive.js
354224848179261915075n
Calculated in 7ms
```

-------

So now we have two reasonably performant functions that we can use in whatever bizarre application it is that calls for us to calculate arbitrary terms of the Fibonacci sequence on the fly instead of using a look-up table!

Buuut…I __*did*__ like the simplicity of that initial recursive implementation though…

Fortunately, we can have our cake and eat it too, thanks to [Tail Call Optimisation](https://exploringjs.com/es6/ch_tail-calls.html) (TCO)! Remember that naive recursive implementation that overflowed the stack? Yeah, well it turns out that language designers (and as a result their implementations) are pretty good at optimising recursive functions **BUT** only if the last thing in the function is a call to itself. 

Remember that before, our function was returning the __sum__ of __two__ calls to itself:

```javascript 
  // ...
  return fibonacci(n-1) + fibonacci(n-2)
}
```

No bueno. What we need to do is to re-write the function so that it’s only returning the result of a single call to itself. We can do this like so:

```javascript
const fibonacci = (n, a=0n, b=1n) => {
  if(n <= 1n) return b
  return fibonacci(n-1n, b, a+b)
}
```

Each call to the function receives:

* __n__ (the current term of the sequence)
* __a__ (the value of the __n-2__th term in the sequence)
* __b__ (the value of the __n-1__th term in the sequence)

If we run this, we should see a vast improvement 🤞, and if we run this for the 100th term of the sequence:

```
$ node ./fibonacci-tail-recursive.js
354224848179261915075n
Calculated in 7ms
```

Huzzah!

So what’s so scary about recursion? ¯\_(ツ)_/¯


-----

Originally posted on [Medium](https://medium.com/@adamhenley/whats-so-scary-about-recursion-24b648544f8)




