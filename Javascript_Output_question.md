**Question 1 :What will the code below output to the console and why?**

```javascript
(function(){
  var a = b = 3;
})();

console.log("a defined? " + (typeof a !== 'undefined'));
console.log("b defined? " + (typeof b !== 'undefined'));
```

Answer :

Since both ```a``` and ```b``` are defined within the enclosing scope of the function, and since the line they are on begins with the var keyword, most JavaScript developers would expect ```typeof a``` and ```typeof b``` to both be undefined in the above example.

However, that is not the case. The issue here is that most developers incorrectly understand the statement ```var a = b = 3;``` to be shorthand for:

```javascript
var b = 3;
var a = b;
```
But in fact, ```var a = b = 3;``` is actually shorthand for:

```javascript
b = 3;
var a = b;
```
As a result (if you are not using strict mode), the output of the code snippet would be:

`a defined? false
b defined? true``
```


