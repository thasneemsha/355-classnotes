*Thasneem Mohamed*
## JS Memory Management 
**Date:** Feb 02, 2026 and **Date:** Feb 11, 2026

---
#### Stack and Heap
JavaScript stores data in two main memory regions:
* **Stack** : Stores primitives, Stores references (addresses) to heap objects, Stores function execution frames
* **Heap** : Stores objects, Stores functions, Stores dynamically allocated data
> Primitives (number, boolean, null, undefined) → stored directly in stack
Objects & functions → stored in heap, stack holds reference
---

#### Primitive Assignment (Pass by Value)
**example 1:** 
```js
let n1 = 31;
let n2 = n1;
n1 = 32;
console.log(n2);
```
 **Step 1** : let n1 = 31; //initialization
 **Step 2** : let n2 = n1;
New memory address created and Values are copied.

| STACK      |         |       |
| ---------- | ------- | ----- |
| identifier | address | value |
| n1         | 0xA1    | 31    |
| n2         | 0xA2    | 31    |

 **Step 3** : let n1 = 32;
Value changes only for n1.

| STACK      |         |           |
| ---------- | ------- | --------- |
| identifier | address | value     |
| n1         | 0xA1    | \31\ → 32 |
| n2         | 0xA2    | 31        |

 **Output**: ```31 ```
Primitive assignment = **pass by value**
---

#### Object Assignment (Copy of Reference)
**example 2:** 
```js
let f1 = {num:3, den:4};
let f2 = f1;
```

#### Step 1

| STACK      |         |       |
| ---------- | ------- | ----- |
| identifier | address | value |
| f1         | 0xB1    | 0xH1  |

| HEAP        |                |
| ----------- | -------------- |
| mem address | value          |
| 0xH1        | {num:3, den:4} |

#### Step 2

`f2` copies the reference (heap address)

|STACK       |         |        |
| ---------- | ------- | ----- |
| identifier | address | value |
| f1         | 0xB1    | 0xH1  |
| f2         | 0xB2    | 0xH1  |

| HEAP        |                |
| ----------- | -------------- |
| mem address | value          |
| 0xH1        | {num:3, den:4} |

Both point to SAME heap object.

---
#### Reassigning Object

```js
f1 = {num:1, den:2};
```
New object created on heap.

| STACK      |         |               |
| ---------- | ------- | ------------- |
| identifier | address | value         |
| f1         | 0xB1    | \0xH1\ → 0xH2 |
| f2         | 0xB2    | 0xH1          |

| HEAP        |                |
| ----------- | -------------- |
| mem address | value          |
| 0xH1        | {num:3, den:4} |
| 0xH2        | {num:1, den:2} |
---
#### Modifying Object Properties
**example 3:** 
```js
let f1 = {num:3, den:4};
let f2 = f1;
f1.num = 1;
f1.den = 2;
console.log(f2)
```
No new heap object created.

| STACK      |         |       |
| ---------- | ------- | ----- |
| identifier | address | value |
| f1         | 0xB1    | 0xH1  |
| f2         | 0xB2    | 0xH1  |

| HEAP        |                        |
| ----------- | ---------------------- |
| mem address | value                  |
| 0xH1        | {num:\3\→1, den:\4\→2} |

### Output:

```
{num:1, den:2}
```
Objects = pass by copy of reference (call by sharing)

---

#### Functions Stored in Heap
**example 4:**
```js
let f1 = {num:3, den:4};
f1.toDecimal = function(){ return this.num / this.den };
```
everything thats not a primitive goes to the Heap
Functions are objects → stored in heap.

| STACK      |         |       |
| ---------- | ------- | ----- |
| identifier | address | value |
| f1         | 0xC1    | 0xH1  |

| HEAP        |                                |
| ----------- | ------------------------------ |
| mem address | value                          |
| 0xH1        | {num:3, den:4, toDecimal:0xH2} |
| 0xH2        | function(){...}                |

---
#### Nested Object
**example 5:**
```js
let f1 = {num:3, den:4, inverse:{num:4, den:3}};
let f2 = {...f1};
f1.inverse.num = 2;
```
Only outer object copied.

| STACK      |         |       |
| ---------- | ------- | ----- |
| identifier | address | value |
| f1         | 0xE1    | 0xH1  |
| f2         | 0xE2    | 0xH2  |

| HEAP        |                              |
| ----------- | ---------------------------- |
| mem address | value                        |
| 0xH1        | {num:3, den:4, inverse:0xH3} |
| 0xH2        | {num:3, den:4, inverse:0xH3} |
| 0xH3        | {num:\4\→2, den:3}           |

Nested object still shared!

---
#### JSON 

```js
let f2 = JSON.parse(JSON.stringify(f1));
```
* Doesn't support functions
* Converts Dates to strings
* Inefficient
* but Structure clone doesn't lose anything
---

## Scope :

Three main scopes (Node.js):

1. **Global**: Variables accessible everywhere, no prefix/keyword. (Avoid this to prevent bugs.)
2. **Function**: Variables declared with `var` or as parameters. They are contained within the function they are created.[not used a lot]
3. **Block (Lexical)**: Variables declared with `let`(allow chnages) or `const`(dont allow chnages), less error, btwn any {}. This is the recommended modern approach.

---

#### Function Scope

```js
function myfunc(a,b){
   var x = 10;
   return a + b;
}
Sum(3,5);
console.log(x) // error as x is inside the function
```
Output: ``` error ```

When function is called: →  New stack frame created.
After return → stack frame removed.
```js
function myfunc(){
   if(false){
      var x = 10;
   }
   console.log(x);
}
```
Output: ``` undefined ```

---
#### Block Scope (need let / const)

```js
{
   var x = 10; //accessible outside
   let y = 20; //not accessible outside{}
}
```
- if you use ```let``` statement then it can't be used in a single statement with no {}

---

#### Higher Order Function
- takes in input as a function
- returns output as a function
- or both

```js
function outer(a){
   let b = 20;
   let unused = 50;
   return function inner(c){
      let d = 40;
      return `${a},${b},${c},${d}`;
   }
}
```

```js
let i = outer(10);
let j = i(30);
console.log(j);
```
- since let i = outer(10) is an initialization with function call it creates a new stack frame and stores each parameters , *return pointers and then goes inside the function body.
- and the next function inner will have a new stack frame again for c, *return ptr and d
---

#### Closure - Preservation of access to variable

When outer returns:
* Its stack frame is removed
* BUT variables used by inner remain in heap
* Garbage collector removes: Variables that are not referenced
```js
function outer(){
   var b = 10;
   var unused = 5;
   function inner(){
      var a = 20;
     console.log(a+b);
   }
   return inner;
}
```
* the above example preserve both a and b. where a is scope and b is in closer.
---

#### not() - Higher Order Function
- takes in input (boolean function).
- outputs the oposite of the function behavior.

```js
function not(func){
   return (...args) => !func(...args);
}

const is_even = x => x % 2 == 0;
const is_odd = not(is_even);
```

Even if:

```js
is_even = null;
not = null;
```

even after making `is _even = null` the  `is_odd` still works because closure preserved reference.

---
#### What is Closure used for 

* Asynchronous Programming
* Hiding Private Members
* Data encapsulation || Creating class-like behavior
* Maintaining state between function calls

#### Hiding Private Members  
Closures allow us to simulate **private variables and methods** in JavaScript.
```javascript
function Dog(n){
    let name = n;
    let age = 0;
    let inventory = [];
    function bark(){
        console.log(`${name} barks!`);
    }
    function birthday(){
        console.log(`Happy Birthday ${name}, you are now ${++age}`);
    }
    function pickup(thing){
        inventory.push(thing);
        console.log(`${name} picked up a ${thing}!`);
        display_inventory();
    }
    function display_inventory(){
        console.log(`${name} has [${inventory.join(", ")}] in their inventory`);
    }
    return { bark, birthday, pickup };
}
let sparky = Dog("Sparky");
let fluffy = Dog("Fluffy");

sparky.birthday();
sparky.birthday();
fluffy.birthday();
sparky.pickup("Bone");
sparky.pickup("Chew Toy");
fluffy.pickup("Food Bowl");
````
* This syntax is very similar to class notation.
* An object containing multiple functions is returned.
```javascript
return { bark, birthday, pickup };
```

##### What Happens Each Time `Dog()` Is Called?

Each call to `Dog()` creates new variables: `name`, `age`, `inventory`that acts like **instance variables**.

Example:

```javascript
let sparky = Dog("Sparky");
let fluffy = Dog("Fluffy");
```

Now:

* `sparky` has its own `name`, `age`, and `inventory`
* `fluffy` has its own separate `name`, `age`, and `inventory`
* They do NOT share data.
* Each returned function (`bark`, `birthday`, `pickup`) forms a **closure**.

A closure: 
> A function that remembers variables from the scope where it was created, even after that outer function has finished executing.

Even after `Dog()` finishes running:`name`,`age`,`inventory`are still accessible to the returned functions. JavaScript keeps them in memory because they are still being referenced.

##### Why This Creates Private Variables
From outside:
```javascript
sparky.name
sparky.age
sparky.inventory
```
These return `undefined`. You cannot access them directly.
The ONLY way to modify them is through:

```javascript
sparky.birthday();
sparky.pickup("Bone");
```

This means: --> Variables are private,  Methods are public
which simulates: Private fields and Public methods

##### Why Not Return the Properties Directly?

if we try:

```javascript
return { name, age, inventory, bark };
```
This does NOT work properly.
Because:

* The `return` statement creates a new object
* Primitive values (`name`, `age`) are copied by value
* The returned object would not stay properly connected to internal state
* Encapsulation would be broken

Instead, we return only methods.
> Using Getters and Setters 
If you need access to values, use functions:

```javascript
function Dog(n){
    let name = n;
    let age = 0;
    function getName(){
        return name;
    }
    function getAge(){
        return age;
    }
    function birthday(){
        age++;
    }
    return { getName, getAge, birthday };
}
```
Now values are still protected but readable through controlled access.

---

