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
