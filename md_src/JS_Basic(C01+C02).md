

## JavaScript_Basic


### Numbers 

JavaScript **does not have integers**.

All numbers are stored as **64-bit floating point numbers**.

Example:

```javascript
console.log(0.2 + 0.1); // 0.30000000000000004
``` 
### Safe Integer Limits

```javascript
console.log(Number.MAX_SAFE_INTEGER);
console.log(Number.MIN_SAFE_INTEGER);
```

These show the **largest and smallest safe integers** JavaScript can handle correctly.

---

### Strings

JavaScript has **three ways to write strings**.

```javascript
let x = 'Normal'; // Single Quotes

let y = "Normal"; // double Quotes

let z = `Template ${1 + 1} ${x.length}`; // Template Strings (Backticks)
 // output : template = 2 6 
```

Template strings allow you to **insert expressions using `${}`**.

---

### Primitive vs Object Version

Every primitive type also has an **object wrapper**.

Example:

```javascript
let a = new String("Helloworld");
// console.log(typeof a); // object
let b = new String("Helloworld");
//console.log(typeof b); // object
```
Comparison:

```javascript
console.log(a == b);   // false
console.log(a === b);  // false
```
> Because **objects are compared by reference (memory location)**.
Even if the values look the same, they are **different objects in memory**.

#### Auto Conversion (Primitive → Object)

Example:

```javascript
console.log("Hello World".length);
```

Even though `"Hello World"` is a primitive string, JavaScript temporarily converts it to a **String object** so we can use `.length`.

But for ***Numbers and Methods*** need parentheses or .. or Number 

Example:

```javascript
console.log((314159).toFixed(2));
```

Because JavaScript thinks this: means **a decimal number** (`314159.`)

---

### Objects

Objects store **key : value pairs**.

Example:

```javascript
const object_02 = {a:3, b:"ABC"};
```

Structure:

```
{
 key : value,
 key : value
}
```

Access values:

```javascript
console.log(object_02.a); // 3
```
---

### Const Objects

Example:

```javascript
const object_08 = {a:"b"};
```

You **cannot reassign** the object:

```javascript
object_08 = {a:"c"}; // ERROR

object_08.a = "c"; // allowed

Object.freeze(object_08); // Now the object cannot be modified.
```
> Because `const` prevents changing the **reference**, not the **contents**.
---

### Object Comparison

Example:

```javascript
const object_05 = {a:3, b:"ABC"};
const object_06 = {a:3, b:"ABC"};

console.log(object_05 == object_06); // false
```

* Objects are compared by **reference**, not by value.

* Each object is stored in a **different memory location**.

---

### Destructuring

Destructuring means **taking values from an object and storing them into variables**.

Example:

```javascript
let object_11 = {a:1, b:2, c:3};

let {a, c} = object_11; // shportcut writing of : let a = object_11.a

// a = 1
// c = 3
```
---

## Functions

Functions in JavaScript are **objects**.

Example:

```javascript
function function_01(a, b){
   return a + b;
}
```

### Function Declaration

Example:

```javascript
function function_02(a, b){
   return a + b;
}

console.log(function_02(1,2)); // 3
```

#### Hoisting

Function declarations are **hoisted**.

>JavaScript **moves the function to the top of the file internally**, so it can be used before it is written.


### Function Expression

Example:

```javascript
const function_03 = function(a,b){ 
   return a+b;  // ReferenceError
};
```
> You **cannot use it before defining it**.

#### Anonymous Functions = **function with no name**

Often used with methods like `.map()`.

Example:

```javascript
let input = [1,2,3,4];

let output = input.map(function(x){
  return x*x; // [1,4,9,16]
});
```
---

### Arrays

Arrays store **multiple values**.

Example:

```javascript
let array_01 = [1, "Two", {three:"value"}, function(){ return 4} ];
```

You can mix types.

To print 4:

```javascript
console.log(array_01[3]());
```

Explanation:

```
array_01[3] → function
() → call the function
```

##### Add element

```javascript
array.push(6);
```

##### Search

```javascript
["A","B","C"].indexOf("C");
```

##### Remove

```javascript
array.splice(index, deleteCount);
```
##### Spread Operator

Example:

```javascript
let array = [1,2,3,4,5];

console.log(Math.max(...array)); // Math.max(1,2,3,4,5)
```

`...` spreads the array into separate values.

---

### undefined vs null

 ***undefined*** = Value **not assigned yet**

Example:

```javascript
let x;
```

***null*** = Value intentionally **empty**

Example:

```javascript
let supervisor = null; // I intentionally have no supervisor
```

---

### Variable Declarations

JavaScript has **4 ways** to declare variables.

| Keyword | Scope    |
| ------- | -------- |
| let     | block    |
| const   | block    |
| var     | function |
| none    | global   |


Temporal Dead Zone

Example:

```javascript
console.log(k); // ReferenceError
let k = 0;
```
--> Because `let` variables **cannot be used before they are declared**.

But this works:

```javascript
let k;
console.log(k); // undefined
k = 0;
```
