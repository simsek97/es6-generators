# ES6 Generators

One of the most interesting features that came out with ES6 is generators. I will have a look at what generators are and how to use them.

In order to understand how they are differnet than normal functions let us have a look at this simple example.

```javascript
function hello() {
  setTimeout(() => {
    console.log('hello');
  }, 3); // 3 miliseconds
}

function count() {
  for (let i=1; i < 500; i++) {
    console.log(i);
  }
}

hello();
count();

// Prints out on console
// 1
// 2
// 3
// ...
// 499
// hello
```

Even though we call `hello()` first and then `count()` we can see hello runs after count as hello can not interrupt count running.
Why is that not possible for hello to interrupt count? The answer is simple. Because Javascript is single-threaded which means only one script can run at any given time.

Generators are coming to the stage at this point. We can have functions with such abilities as we can run, pause, and run again later.

ES6 generator functions hava a magic keyword called `yield` that enables itself to pause itself which is amazing.

Another amazing feature of generator functions is that beyond pausing and reruning the function, we can also get and send messages as it progresses.

# Generator Functions
