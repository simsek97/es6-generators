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

# Generator Functions

ES6 generator functions hava a magic keyword called `yield` that enables itself to pause itself which is amazing.

Another amazing feature of generator functions is that beyond pausing and reruning the function, we can also get and send messages as it progresses.

Generator functions are defined like below

```javascript
function* count() {
}
```

or

```javascript
function *count() {
}
```
and whenever we'd like to pause the function and send a message, we use ``yield`` keyword as below

```javascript
function* count() {
    yield "counting..."
}
```

In practice, this will do nothing. In order to use generator functions, we need a method called iterator.

```javascript
var it = count();
```
is an iterator. And when we log it we will see something interesting

```javascript
console.log(it);

// Prints out
// {next: ƒ, throw: ƒ, return: ƒ}
```

Quite interesting? Huh! This is an object which has 3 functions. Let's call these functions.

```javascript
var it = count();
console.log( it.next() );

// {value: "counting", done: false}
```

```javascript
var it = count();
console.log( it.throw() );

// {value: "counting", done: false}
```

```javascript
var it = count();
console.log( it.return() );

// {value: undefined, done: true}
```

The most exciting part here is that we are getting an object with ``value: counting`` which is the ``yield`` part of our function.

What happends if we have more yields. Have a look.

```javascript
function* count() {
    yield 1
    yield 2
    yield 3
}

var it = count();
console.log( it.next() );
// {value: 1, done: false}

console.log( it.next() );
// {value: 2, done: false}

console.log( it.next() );
// {value: 3, done: false}

console.log( it.next() );
{value: undefined, done: true}
```

So, each time we call ``next()`` we are getting the next ``yield`` and one more interesting thing is that ``done`` is ``true`` when we call ``next()`` after all ``yield``s. 

These all mean we are able to get messages from the function.

What if we want to send messages?

```javascript
function* count() {
    a = yield "Send me a number";
    console.log("You sent me ", a);
    
    console.log("I will send you back ", (a + 5));
    
    b = yield a + 5;
    console.log("You sent me ", b);
    
    console.log("I will send you back 3");
    yield 3

    console.log("I am done. Returning ", b + 1);
    return b + 1;
}

var it = count();

console.log( it.next() );
// {value: "Send me a number", done: false}

console.log( it.next(3) );
// You sent me 3
// I will send you back 8
// {value: 8, done: false}

console.log( it.next(1) );
// You sent me 1
// I will send you back 3
// {value: 3, done: false}

console.log( it.next() );
// I am done. Returning 2
// {value: 2, done: true}
```

I will end with another couple of examples.

```javascript
function* count() {
    var a = yield "Enter a number!";
    console.log("You entered ", a, " and you will get", a, " + 5 = ", (a + 5));

    var b = yield a + 5;
    var c = a * 3 + b;
    console.log("You sent me", b, " and you will get (", a, " * 3) + ", b, "=", c);

    return c;
}

var it = count();
console.log( "First call:", it.next() );
console.log( "Second call:", it.next(3) );
console.log( "Third call:", it.next(1) );

// Prints out these
// First call: {value: "Enter a number!", done: false}
// You entered 3 and you will get 3 + 5 = 8
// Second call: {value: 8, done: false}
// You sent me 1 and you will get (3 * 3) + 1 = 10
// Third call: {value: 10, done: true}
```

```javascript
function *count() {
    console.log(yield);
    console.log(yield);
    console.log(yield);
    console.log(yield);
    console.log(yield);
}

const it = count();
let i = 0;
while (!it.next(i * 10).done) {
    i++;
    console.log(i);
}

// Print out these
// 1
// 1
// 2
// 4
// 3
// 9
// 4
// 16
// 5
// 25
```
