# Default / Rest / Spread as Function Arguments

## Learning Goals

- Set a default value for a function parameter
- Use JavaScript's `rest` parameter to define parameters in a function
- Use JavaScript's `spread` operator in a function call

## Introduction

The typical way to handle function parameters for JavaScript functions is to
define them individually and then pass a value for each as an argument when
calling the function:

```js
function customGreeting(greeting, name, adjective, occasion) {
  return `${greeting}, ${name}! Have a ${adjective} ${occasion}!`;
}

customGreeting("Good morning", "Pouja", "fantastic", "Tuesday");
// => "Good morning, Pouja! Have a fantastic Tuesday!"
```

This always works, and is perfectly adequate a lot of the time. However, for
those times when our needs may be more complex, JavaScript provides several helpful
tools for handling parameters and arguments that can make our code more flexible
and efficient: default values, the `rest` parameter and the `spread` operator.
We will learn how to use all three in this lesson.

## Set a Default Value for a Function Parameter

Let's say you work for an e-commerce site, and you're prepping for your
post-holiday sales. You're working on some code for your website and you need to
set a discount of 25% across the board for everything that you sell on the
website.

We can create a function that takes in an `itemPrice` as a price in dollars and
returns the discounted price:

```js
function discountedPrice(itemPrice){
    return itemPrice - (itemPrice * 0.25);
}
```

But it seems unwise to hard-code the discount to `0.25`. Management's whims on
discount rates change almost daily, as the corporate sales office's machine-learning
algorithms recommend new discount rates. Because of this, we want to encode the
discount rate as a _parameter_ of the function.

```js
function discountedPrice(itemPrice, discount){
    return itemPrice - (itemPrice * discount);
}
```

So, calls to `discountedPrice()` will look like:

```js
function discountedPrice(itemPrice, discount){
    return itemPrice - (itemPrice * discount);
}
discountedPrice(100, 0.25); //=> 75.0
```

But it _also_ seems a bit of a burden to **have** to pass the discount amount on
every call. We'd like `discount` to _default_ to `0.25`. It'll be 25% off
_unless_ we choose to pass a new discount percentage into `discountedPrice()`. We
set the default value by simply assigning the value in the parameter list:

```js
function discountedPrice(itemPrice, discount = 0.25){
    return itemPrice - (itemPrice * discount);
}
discountedPrice(100); //=> 75.0
```

Then if we want to use a different discount, we simply pass the new value as the
second argument — that value will _override_ the default value:

```js
discountedPrice(100, 0.5); //=> 50.0
```

Note that we place the parameter with a default value at the **end** of the
parameter list. This is because the argument values are assigned to the
parameters from left to right, regardless of whether a default value is
specified for any of them. If we put the discount with its default value first
and `itemPrice` second, when we call `discountedPrice(100)`, 100 will get
assigned as the discount and the `itemPrice` will be undefined. The function
would then return `NaN`:

```js
function discountedPrice(discount = 0.25, itemPrice) {
  return itemPrice - (itemPrice * discount);
}

discountedPrice(100);
// => NaN
```

Now say we want to add a `tax` parameter to `discountedPrice()` so that we can
include a tax percentage to be added to the discounted sales price. First we
put our function declaration back the way it was, with `discount = .25` last.
Then we add `tax` as the second parameter:

```js
function discountedAndTaxedPrice(itemPrice, tax, discount = 0.25) {
  let subtotal = itemPrice - (itemPrice * discount);
  return subtotal + (subtotal * tax);
};

discountedAndTaxedPrice(100, 0.10); //=> 82.5
discountedAndTaxedPrice(100, 0.10, 0.20); //=> 88
```

## Use JavaScript's `rest` Parameter to Define Parameters in a Function

You might have heard a little bit about JavaScript's magical "three dots". These
three dots can be used in two very different ways — as the `rest` parameter and
as the `spread` operator. The `rest` parameter allows you to collect the `rest`
of your remaining arguments that you are passing into your function into an
array, while the `spread` operator allows you to pass elements of an array into
a function as an argument.

Sometimes, we might not know exactly how many arguments we want to pass into a
function, but we might know that we only want to do something with the first two
arguments. In JavaScript, it's possible to pass in any number of arguments into
a function when we call it, regardless of how many parameters are defined:

```js

function muppetLab(a,b){
  console.log(a,b); // LOG: Dr. Bunson Beaker
}

muppetLab("Dr. Bunson", "Beaker", "Miss Piggy", "Kermit", "Animal");
```

Here we have two parameters defined, so the first two arguments are stored into
those variables. But what happens if we want to capture the left over arguments?
The `rest` parameter allows us to take the rest of the arguments that we pass in
to the function, regardless of how many there are, and gather them into an
array. Here's how this works:

```js
function muppetLab(a, b, ...muppets) {
  console.log(a, ' ', b); // LOG: Dr. Bunson Beaker

  console.log(muppets); // LOG: ["Miss Piggy", "Kermit", "Animal"]
  console.log(muppets[0]); // LOG: Miss Piggy
  console.log(muppets.length); // LOG: 3
}

muppetLab("Dr. Bunson", "Beaker", "Miss Piggy", "Kermit", "Animal");
```

If we call `muppetLab()` and only pass two arguments, the value of `muppets`
will be an empty array.

Since the `rest` parameter gathers the rest of the arguments given to a
function, it should always come at the end of the list of parameters.

## Use JavaScript's `spread` Operator in a Function Call

With the `rest` operator, JavaScript allowed us to put the remaining arguments
into an array. The `spread` operator allows us to pass elements of an array into
a function as an argument. Try it out in console with a simple add function:

```js
function add(a, b, c) {
  return a + b + c ;
}
const arr = [1, 2, 3];

add(...arr); // returns 6
```

So what's happening here? We have a simple add function, with three arguments,
and we are passing in an array using the `spread` operator. The function is
adding all three numbers contained within the array. Play around with it using a
bigger array and see what happens when the array has more numbers than our
function has parameters.

## Conclusion

Since the syntax here is similar, how do we know when JavaScript is using the
`spread` operator and when it's using the `rest` parameter? It's all about
context. If the three dots occur when you are _calling_ the function, then it's
the `spread` operator. If they happen when you're _defining_ the function, it's
the `rest` parameter. Don't forget to use default parameters when you have an
argument that you are going to be defining on a regular basis to increase your
code efficiency!

## Resources

- [Default parameters][]
- [Rest parameters][]
- [Spread operators][]

[Default parameters]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters
[Rest parameters]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters
[Spread operators]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax
