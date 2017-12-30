Topics:  
1. [Warm-Up](#warm-up)  
2. [Introduction](#introduction)  
3. [Default Arguments ES5:](#default-arguments-es5)  
4. [Default Arguments ES6:](#default-arguments-es6)  
5. [Practice:](#practice)

#### Warm-Up  
We’re starting with a little warm-up:
```  
  function getName1() {  
      return {
          name: "Best production team ever!"
      }
  }
```

```  
  function getName2() {
      return
      {
          name: "Worst production team ever!"
      }
  }  
```
Given these two functions, what would be logged on the console after running:
```  
  console.log(getName1().name);  
```
```  
  console.log(getName2().name);
```

In JavaScript semi-colons are optional. When the JavaScript's engine reads one character at a time, it knows what to expect to. When it reads `return`, it knows that either it accepts a returned value, or returning nothing - which leads to an automatically semi-colon insertion.  
* At function `getName1()`, after the return key-word we wrote a starting `object literal` with a `{`.  
* At function `getName2()` there is an end-line after the `return`, so JavaScript engine inserts automatically a semi-colon. In this case, the whole object literal is unreachable code.

## Introduction  
As in many other languages, JavaScript has its own way to declare default values for the arguments which passed to a function.  
This may help us for some cases:  
* Avoiding situations which we don't know if a value would be passed to an argument, and the value would be undefined.
* Making our code much more readable.

## Default Arguments ES5:
In ES5, there isn't a real syntactic way to set default arguments, and there are some ways we can do that, and here I'll represent three of them:  
A very common way to get default arguments in ES5, is working with JavaScript coercion:
```  
  function ES5defaultParams(i, j) {
      i = i || 5;
      j = j || 6;
  }
```
* Advantages: Readable and short.  
* Disadvantages: `“”` (an empty string) and `0` are coerced into false, so when we really need to pass these values, the argument would receive the chosen default value.

Example:  
```  
  function logAfterX(x) {
      x = x || 2000;
      setTimeout(function () {
          console.log("logging...");
      } , x);
  }
```
When we will pass `0` as an argument, the timeout would be 2000 ms.

##### Solution 1:
Checking for each argument if received a value:
```  
  function ES5defaultParams(i, j) {
      i = typeof i !== 'undefined' ? i : 5;
      j = typeof j !== 'undefined' ? j : 6;
  }
```

* Advantage: Works for every case.  
* Disadvantage: A little long.

##### Solution 2:
`Lodash`!  
With `_.defaults()` we can set for an object default key-value pair values:
```  
  function lodashDefaultObject (objArg) {
      _.defaults(objArg, {arg: "My Default value"})
  }
```
* Advantage: Very readable and short.  
* Disadvantage: Works for objects only.  


## Default Arguments ES6:
#### Default Variables:
In ES6 we have a very readable syntax which allows us declaring default values for function’s arguments:
```  
  function multiply(a, b = 2) {
      return a * b;
  }
```

```  
  console.log(multiply(5)); // 10
  console.log(multiply(3,6)); // 18
  console.log(multiply()); // NaN
```
Why is the third a NaN? Because we haven’t passed or set any value \ default value.  

Very simple ha? Lets get things a little more complicated! :)

#### Default Objects:
In order to understand how default arguments as objects works on ES6, we should understand what is `Destructuring Assignment`.  
__Destructuring Assignment__ - is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.  
Notice that it sais -distinct- which is a very meaningful word here. It actually refers to the excact key in the object.  

Example 1:
```  
  ({ a, c } = { a: 10, c: 20 });
  console.log(a); // 10
  console.log(c); // 20
```
As you can see, the name of the variable has to be exactly the same as key in the given object.

Example 2:  
```  
  const x = [1, 2, 3, 4, 5];
  let [y, z] = x;
  console.log(y); // 1
  console.log(z); // 2
```
When receiving values from array, it would assign values by order.

Example 3:  
```
  let [a = 5, b = 7] = [1];
  console.log(a); // 1
  console.log(b); // 7
```
Here we can see a default value of b, which assigned because no value is declared for b.

Example 4:  
```
  let a = 2, b = 3;
  [a, b] = [b, a];
  console.log(a); // 3
  console.log(b); // 2
```
A useful way to swap with destructuring assignment.


So, what destructuring assignment has to do with default arguments?  
Lets do it step by step:  
* First, declare an object with destructuring assignment:  
    ```  
  function func(myObject = {}) {

  }
    ```

* Then, we can declare two keys (variables) which would gain `myObject` key’s values.  
    ```  
  function func(myObject = {}) {
      let { arg1, arg2 } = myObject;
  }
    ```
Notice that arg1 and arg2 must be the same keys as myObject has! Hence, the order doesn’t really matters.

* At last, you are allowed assign default values for each key:  
    ```  
  function func({ arg1 = 0, arg2 = 50 } = {}) {

  }
    ```
Readable, isn’t it?
Try it yourself!

### Practice:  
Write a function which invokes setTimeout:
   ```  
 /*
 *  time - Number argument or default value 2000
 *  f - callback function or a function which logs by default 'waited {time} ms'
 */
func( time , f {function} )
 ```
Solution 1: (with default values)  
```
  function func(wait = 2000, f = function () {
      console.log("finished timeout");
  }) {
      setTimeout(f, wait);
  }
```
Invoking:  
```
func(10, myCallback);
```

Solution 2: (with default objects)  
```
  function func({wait = 2000, f = function () {
      console.log("finished timeout");
  }} = {}) {
      setTimeout(f, wait);
  }
```
Invoking:  
```
  func({wait: 10, f: myCallback});
```