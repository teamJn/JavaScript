Q. Is there any difference between following two lines of code?

```javascript
var a = new Array(2);
var a = new Array("2");
```

A. First one creates an array of length 2. Second one creates an array with one element "2".

Q. What will be the output of following code snippet?

```javascript
var a = ["Apple", "Banana"];
a["1"] = "Orange";
console.log(a);
```

A. `["Apple", "Orange"]`

Q. How `every()` and `reduce()` methods of Array differ?
A. `every()` stops execution when it finds first falsy value. `reduce()` completes the loop.
