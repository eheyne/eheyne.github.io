---
layout: post
title:  "ES6 Arrow Functions"
date:   2016-7-7
---

<p class="intro"><span class="dropcap">I</span>n my endeavor to learn the new features of ECMAScript 6, I began digging into Arrow functions.  At a first glance, it appeared that they were just a simpler syntax for anonymous functions.  After researching them, I learned that there are many more aspects to Arrow functions.  So here are my findings on ES6 Arrow functions.</p>

<h4>Arrow Functions in place of Anonymous Functions</h4>
First about using arrow functions as anonymous functions.  This first example shows an arrow function used in an Expression body.

On occasion in writing JavaScript code we find ourselves writing expressions that require a function as an argument.  So we might write the following JavaScript in ES5:

{% highlight javascript %}
var salaries = [40000, 50000, 60000];

var salariesWithBonus = 
  salaries.map(function(salary) {
    return salary + (salary * .10);
  });
// salariesWithBonus is now equal to [44000, 55000, 66000]
{% endhighlight %}

In ES6, in place of an anonymous function we can use an Arrow function.  To demonstrate the use of an expression body, we have the following code:

{% highlight javascript %}
var salaries = [40000, 50000, 60000];

var salariesWithBonus = 
  salaries.map(salary => {
    return salary + (salary * .10);
  });
// salariesWithBonus is now equal to [44000, 55000, 66000]
{% endhighlight %}

If we needed to pass in more than one argument into our anonymous function, you would want to surround the arguments with parenthesis, like the following sample code:

{% highlight javascript %}
var salaries = [40000, 50000, 60000];

var runningTotal = 
  salaries.map((salary, index, array) => {
    if (index === 0) return salary;

    var s = 0;
    for (var i = 0; i <= index; i++) {
      s += array[i];
    }
    
    return s;
  });
// runningTotal is now equal to [40000, 90000, 150000]
{% endhighlight %}

<h4>Implicit Returns</h4>
If an arrow function body contains a single line of code, then you can exclude the beginning and ending brackets as well as the return statement.  The following functions are the same:

{% highlight javascript %}
var salaries = [40000, 50000, 60000];

// ES5
var salariesWithBonus = 
  salaries.map(function(salary) {
    return salary + (salary * .10);
  });

// ES6
var salariesWithBonus = 
  salaries.map(salary => salary + (salary * .10));

// salariesWithBonus is [44000, 55000, 66000] in both cases
{% endhighlight %}

<h4>What does 'this' refer to in Arrow Functions</h4>
When using arrow functions what value does the `this` pointer contain?  Let's take a look at another example and see what the value is.  This first code example shows a constructor function that defines a property assigned the value of 0.  We then see a setTimeout that will accept a function that will increment the property by 1.  This exmaple is using ES5 syntax.

{% highlight javascript %}
function TestObj() {
  this.count = 0;
  setTimeout(function AddOne() {
    console.log('this is', this);
    this.count += 1;
  }, 1000);
}

var t = new TestObj();

// this is Window and t.count has the value of 0.
{% endhighlight %}

The output of this function is given in the comment below it.  We see that the `this` value is `Window`, which does not contain a property `count`, therefore cannot increment it.  So the output is the value of 0 because that's what we initialized it to.  Lets write the same function using an arrow function in the setTimeout.

{% highlight javascript %}
function TestObj() {
  this.count = 0;
  setTimeout(() => {
    console.log('this is', this);
    this.count += 1;
  }, 1000);
}

var t = new TestObj();

// this is TestObj and t.count has the value of 1.
{% endhighlight %}

Once again the output is placed in a comment after the function.  Notice now that `this` refers to the TestObj object, therefore is able to increment the count that is scoped to this context.  With arrow functions, `this` maintains the enclosing context.

<h4>Arrow Functions with apply() and call()</h4>
Since `this` is already refering to the containing context, when trying to call an arrow function using `call` or `apply` you will not be able to change what `this` refers to.

<h4>Arguments in Arrow Functions</h4>
With arrow functions the value of the `arguments` property will no longer contain the list of arguments but instead will be undefined.  Let's take a look at our salary running total example in ES5 again, but this time we will pay attention to the value of `arguments`.

{% highlight javascript %}
var salaries = [40000, 50000, 60000];

var runningTotal = 
  salaries.map(function(salary, index, array) {
    if (index === 0) return salary;
    console.log('arguments is', arguments);
    var s = 0;
    for (var i = 0; i <= index; i++) {
      s += array[i];
    }
    
    return s;
  });
  // arguments is [50000, 1, Array[3]]
  // arguments is [60000, 2, Array[3]]
{% endhighlight %}

In ES6 using arrow functions:

{% highlight javascript %}
var salaries = [40000, 50000, 60000];

var runningTotal = 
  salaries.map((salary, index, array) => {
    if (index === 0) return salary;
    console.log('arguments is', arguments);
    var s = 0;
    for (var i = 0; i <= index; i++) {
      s += array[i];
    }
    
    return s;
  });
  // Uncaught ReferenceError: arguments is not defined
{% endhighlight %}

The `arguments` property is not even defined.

<h4>Arrow Functions as Constructor Functions</h4>
Arrow functions may not be used for constructor functions.

<h4>Conclusion</h4>
So in conclusion arrow functions are a simpler way to write anonymous functions but be careful you understand that what values of `this` and `arguments` will be inside of the arrow function will now be the contained context instead of `Window`.
