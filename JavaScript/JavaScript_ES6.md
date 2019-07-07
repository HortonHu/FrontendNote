支持ES6的浏览器版本号

| Chrome 58   | Edge 14     | Firefox 54 | Safari 10 | Opera 55 |
| ----------- | ----------- | ---------- | --------- | ---------|
| Jan 2017    | Aug 2016    | Mar 2017   | Jul 2016  | Aug 2018 |


# let
The let statement allows you to declare a variable with block scope.
```
var x = 10;
// Here x is 10
{ 
  let x = 2;
  // Here x is 2
}
// Here x is 10
```

# const
Constants are similar to let variables, except that the value cannot be changed.
```
var x = 10;
// Here x is 10
{ 
  const x = 2;
  // Here x is 2
}
// Here x is 10
```


# Exponentiation Operator(**)
The exponentiation operator (**) raises the first operand to the power of the second operand.
```
var x = 5;
var z = x ** 2;          // result is 25
```


# Default Parameter Values
```
function myFunction(x, y = 10) {
  // y is 10 if not passed or undefined
  return x + y;
}
myFunction(5); // will return 15
```


# Array.find()
The find() method returns the value of the first array element that passes a test function.
```
var numbers = [4, 9, 16, 25, 29];
var first = numbers.find(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```


# Array.findIndex()
The findIndex() method returns the index of the first array element that passes a test function.
```
var numbers = [4, 9, 16, 25, 29];
var first = numbers.findIndex(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```


# Number Properties
ES6 added the following properties to the Number object:
- EPSILON
- MIN_SAFE_INTEGER
- MAX_SAFE_INTEGER


# Number Methods
- Number.isInteger()
- Number.isSafeInteger()
```
Number.isInteger(10);        // returns true
Number.isInteger(10.5);      // returns false
```

A safe integer is an integer that can be exactly represented as a double precision number.
The Number.isSafeInteger() method returns true if the argument is a safe integer.
Safe integers are all integers from -(253 - 1) to +(253 - 1).
This is safe: 9007199254740991. This is not safe: 9007199254740992.
```
Number.isSafeInteger(10);    // returns true
Number.isSafeInteger(12345678901234567890);  // returns false
```


# Global Methods
- isFinite()
- isNaN()
The global isFinite() method returns false if the argument is Infinity or NaN.
```
isFinite(10/0);       // returns false
isFinite(10/1);       // returns true
```
The global isNaN() method returns true if the argument is NaN. Otherwise it returns false:
```
isNaN("Hello");       // returns true
```


# Arrow Functions
Arrow functions allows a short syntax for writing function expressions.
You don't need the function keyword, the return keyword, and the curly brackets.
```
// ES5
var x = function(x, y) {
   return x * y;
}

// ES6
const x = (x, y) => x * y;
```
Arrow functions do not have their own this. They are not well suited for defining object methods.
Arrow functions are not hoisted. They must be defined before they are used.
Using const is safer than using var, because a function expression is always constant value.
You can only omit the return keyword and the curly brackets if the function is a single statement. Because of this, it might be a good habit to always keep them:
```
const x = (x, y) => { return x * y };
```






