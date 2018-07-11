**Que 1. What is a potential pitfall with using typeof bar === "object" to determine if bar is an object? 
How can this pitfall be avoided?**

Although typeof ```javascript bar === "object"``` is a reliable way of checking if bar is an object, the surprising gotcha in JavaScript is that null is also considered an object!

Therefore, the following code will, to the surprise of most developers, log true (not false) to the console:

```javascript
var bar = null;
console.log(typeof bar === "object");  // logs true!
```
As long as one is aware of this, the problem can easily be avoided by also checking if bar is null:

```javascript
console.log((bar !== null) && (typeof bar === "object"));  // logs false
```
To be entirely thorough in our answer, there are two other things worth noting:

First, the above solution will return false if bar is a function. In most cases, this is the desired behavior, but in situations where you want to also return true for functions, you could amend the above solution to be:

```javascript
console.log((bar !== null) && ((typeof bar === "object") || (typeof bar === "function")));
```
Second, the above solution will return ```true``` if ```bar``` is an array (e.g., ```if var bar = [];```). In most cases, this is the desired behavior, since arrays are indeed objects, but in situations where you want to also false for arrays, you could amend the above solution to be:

```javascript
console.log((bar !== null) && (typeof bar === "object") && (toString.call(bar) !== "[object Array]"));
```
However, there’s one other alternative that returns ```false``` for nulls, arrays, and functions, but true for objects:

```javascript
console.log((bar !== null) && (bar.constructor === Object));
```

**Que 2. What is the significance of, and reason for, wrapping the entire content of a JavaScript source file in a function block?**

This is an increasingly common practice, employed by many popular JavaScript libraries (jQuery, Node.js, etc.). This technique creates a closure around the entire contents of the file which, perhaps most importantly, creates a private namespace and thereby helps avoid potential name clashes between different JavaScript modules and libraries.

This technique creates a closure around the entire contents of the file which creates a private namespace and thereby helps avoid potential name clashes between different JavaScript modules and libraries. Closure is a mechanism to declare "private" variables in JavaScript.

**Que3 What is the significance, and what are the benefits, of including 'use strict' at the beginning of a JavaScript source file?**

The short and most important answer here is that use strict is a way to voluntarily enforce stricter parsing and error handling on your JavaScript code at runtime. Code errors that would otherwise have been ignored or would have failed silently will now generate errors or throw exceptions. In general, it is a good practice.

> The "use strict" directive is only recognized at the beginning of a script or a function.


Some of the key benefits of strict mode include:

**Makes debugging easier.** Code errors that would otherwise have been ignored or would have failed silently will now generate errors or throw exceptions, alerting you sooner to problems in your code and directing you more quickly to their source.

**With strict mode, you can not, for example, use undeclared variables.**

Strict mode is declared by adding "use strict"; to the beginning of a script or a function.Declared at the beginning of a script, it has global scope (all code in the script will execute in strict mode):

```javascript
"use strict";
x = 3.14;       // This will cause an error because x is not declared
```

Declared inside a function, it has local scope (only the code inside the function is in strict mode):

```javascript
x = 3.14;       // This will not cause an error. 
myFunction();

function myFunction() {
   "use strict";
    y = 3.14;   // This will cause an error
}
```

**Prevents accidental globals.** Without strict mode, assigning a value to an undeclared variable automatically creates a global variable with that name. This is one of the most common errors in JavaScript. In strict mode, attempting to do so throws an error.

**In normal JavaScript, a developer will not receive any error feedback assigning values to non-writable properties.**
In strict mode, any assignment to a non-writable property, a getter-only property, a non-existing property, a non-existing variable, or a non-existing object, will throw an error.

```javascript
"use strict";
var obj = {};
Object.defineProperty(obj, "x", {value:0, writable:false});

obj.x = 3.14;            // This will cause an error
```

**Throws error on invalid usage of delete.** The delete operator (used to remove properties from objects) cannot be used on non-configurable properties of the object. Non-strict code will fail silently when an attempt is made to delete a non-configurable property, whereas strict mode will throw an error in such a case.

Deleting a variable (or object) is not allowed.

```javascript
"use strict";
var x = 3.14;
delete x;`javascript

```
**Disallows duplicate parameter values.** Strict mode throws an error when it detects a duplicate named argument for a function (e.g., function foo(val1, val2, val1){}), thereby catching what is almost certainly a bug in your code that you might otherwise have wasted lots of time tracking down.

```javascript
"use strict";
function x(p1, p2) {}; 

function x(p1, p2) {};  // This will cause an error 
```

**Que 4. What is NaN? What is its type? How can you reliably test if a value is equal to NaN?**

The NaN property represents a value that is “not a number”. This special value results from an operation that could not be performed either because one of the operands was non-numeric (e.g., "abc" / 4), or because the result of the operation is non-numeric.

While this seems straightforward enough, there are a couple of somewhat surprising characteristics of NaN that can result in hair-pulling bugs if one is not aware of them.

For one thing, although NaN means “not a number”, its type is, believe it or not, Number:

```javascript
console.log(typeof NaN === "number");  // logs "true"
```
Additionally, NaN compared to anything – even itself! – is false:

```javascript
console.log(NaN === NaN);  // logs "false"
```
A semi-reliable way to test whether a number is equal to NaN is with the built-in function isNaN(), but even using isNaN() is an imperfect solution.

>A better solution would either be to use value !== value, which would only produce true if the value is equal to NaN. Also, ES6 offers a new Number.isNaN() function, which is a different and more reliable than the old global isNaN() function.


**Que 5 What is a “closure” in JavaScript? Provide an example.**

A closure is an inner function that has access to the variables in the outer (enclosing) function’s scope chain. The closure has access to variables in three scopes; specifically: (1) variable in its own scope, (2) variables in the enclosing function’s scope, and (3) global variables.

Here is an example

```javascript
var globalVar = "xyz";

(function outerFunc(outerArg) {
    var outerVar = 'a';
    
    (function innerFunc(innerArg) {
    var innerVar = 'b';
    
    console.log(
        "outerArg = " + outerArg + "\n" +
        "innerArg = " + innerArg + "\n" +
        "outerVar = " + outerVar + "\n" +
        "innerVar = " + innerVar + "\n" +
        "globalVar = " + globalVar);
    
    })(456);
})(123);
```
In the above example, variables from ```innerFunc```, ```outerFunc```, and the global namespace are all in scope in the innerFunc. The above code will therefore produce the following output:

```
outerArg = 123
innerArg = 456
outerVar = a
innerVar = b
globalVar = xyz
```

**Que 6 What is the value of typeof undefined == typeof NULL?**

The expression will be evaluated to true, since NULL will be treated as any other undefined variable.

Note: JavaScript is case-sensitive and here we are using NULL instead of null.


**Que7 How do you clone an object?**

```javascript
var obj = {a: 1 ,b: 2}
var objclone = Object.assign({},obj);
```

Now the value of objclone is {a: 1 ,b: 2} but points to a different object than obj.

Note the potential pitfall, ***though: Object.clone() will just do a shallow copy, not a deep copy.*** This means that nested objects aren’t copied. They still refer to the same nested objects as the original:

```javascript
let obj = {
    a: 1,
    b: 2,
    c: {
        age: 30
    }
};

var objclone = Object.assign({},obj);
console.log('objclone: ', objclone);

obj.c.age = 45;
console.log('After Change - obj: ', obj);           // 45 - This also changes
console.log('After Change - objclone: ', objclone); // 45
```



