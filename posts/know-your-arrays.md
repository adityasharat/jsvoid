I find the <code class="jsv-fs-inherit">for</code> loop very ugly, especially if it is trying to execute a complicated piece of code. Inside the  <code class="jsv-fs-inherit">for</code> loop the code is less readable, it becomes difficult to debug, and it is less modular as the body of the <code class="jsv-fs-inherit">for</code> loop may not be reusable everytime.

I prefer the native methods of an <code class="jsv-fs-inherit">Array</code> instead of the <code class="jsv-fs-inherit">for</code> loop to perform common tasks like transforming, filtering or aggregating data.

By using these methods you:

* Makes your code more readable.
* You concentrate on *what* it does and stop worrying about the *index*. ( I hate doing <code class="jsv-fs-inherit">[i]</code> :|)
* The intent of your code becomes clearer.
* The code becomes modular and reusable.
* Write *less* to do *more*.

And I want to justify this with a few examples.

#### <code class="jsv-fs-inherit">forEach</code>

The <code class="jsv-fs-inherit">forEach()</code> method executes a provided function once per array element.

```language-javascript
var arr = [{
  name: 'john'
}, {
  name: 'mary'
}, {
  name: 'jane'
}, {
  name: 'peter'
}];
```

I want to capitlize all the names in the array

```language-javascript
// A for loop first
var i;
for(i = 0; i < arr.length; i++) {
  arr[i].name = arr[i].name.substring(0, 1).toUpperCase() + arr[i].name.substring(1, arr[i].name.length);
}
```

```language-javascript
// Let's try the forEach now
function capitalize(person) {
  person.name = person.name.substring(0, 1).toUpperCase() + person.name.substring(1, person.name.length);
}
arr.forEach(capitalize);
```

The <code class="jsv-fs-inherit">forEach()</code> pattern is easier to read, the intent is clear and you can re-use the <code class="jsv-fs-inherit">capitalize</code> method.

I feel that this is even better than following implementation.

```language-javascript
for(i = 0; i < arr.length; i++) {
  capitalize(arr[i]);
}
```

Now I'm going alphabetically

#### <code class="jsv-fs-inherit">concat</code>

The <code class="jsv-fs-inherit">concat()</code> method returns a new array comprised of the array on which it is called joined with the array(s) and/or value(s) provided as arguments.

```language-javascript
var array1 = [1,2,3,4,5],
    array2 = [6,7,8,9,10],
    array3;

array3 = array1.concat(array2);
// [1,2,3,4,5,6,7,8,9,10]
```

Remember that the original array <code class="jsv-fs-inherit">array1</code> will not be modified.

#### <code class="jsv-fs-inherit">every</code>

The <code class="jsv-fs-inherit">every()</code> method tests whether all elements in the array pass the test implemented by the provided function.

```language-javascript
function isNegative(element) {
  return element < 0;
}
[-12, -5, -8, -130, -44].every(isNegative);   // true
[-12, -54, 18, -130, -44].every(isNegative); // false
```

#### <code class="jsv-fs-inherit">filter</code>

The <code class="jsv-fs-inherit">filter()</code> method creates a new array with all elements that pass the test implemented by the provided function. Let's say that I want to find all the people whose name have the letter 'e' in them.

```language-javascript
function hasTheLetterE(person) {
  return (person.name || '').toLowerCase().search(/e/i) > -1;
}
var peopleWithEInTheirName = arr.filter(hasTheLetterE);
```

<code class="jsv-fs-inherit">peopleWithEInTheirName</code> will be an array with Jane and Peter.

#### <code class="jsv-fs-inherit">indexOf</code>

The <code class="jsv-fs-inherit">indexOf()</code> method returns the first index at which a given element can be found in the array, or -1 if it is not present.

```language-javascript
var array = [2, 5, 9];
array.indexOf(2);     // 0
array.indexOf(7);     // -1
```

#### <code class="jsv-fs-inherit">join</code>

The <code class="jsv-fs-inherit">join()</code> method joins all elements of an array into a string.

```language-javascript
var a = ['Wind', 'Rain', 'Fire'];
var myVar1 = a.join();      // assigns 'Wind,Rain,Fire' to myVar1
var myVar2 = a.join(', ');  // assigns 'Wind, Rain, Fire' to myVar2
var myVar3 = a.join(' + '); // assigns 'Wind + Rain + Fire' to myVar3
```

#### <code class="jsv-fs-inherit">lastIndexOf</code>

The <code class="jsv-fs-inherit">lastIndexOf()</code> method returns the last index at which a given element can be found in the array, or -1 if it is not present. The array is searched backwards, starting at fromIndex.

```language-javascript
var array = [2, 5, 9, 2];
array.lastIndexOf(2);     // 3
array.lastIndexOf(7);     // -1
array.lastIndexOf(2, 3);  // 3
array.lastIndexOf(2, 2);  // 0
array.lastIndexOf(2, -2); // 0
array.lastIndexOf(2, -1); // 3
```

#### <code class="jsv-fs-inherit">map</code>

The <code class="jsv-fs-inherit">map()</code> method creates a new array with the results of calling a provided function on every element in this array. It's very handy for transforming data.

```language-javascript
var numbers = [1, 4, 9];
var roots = numbers.map(Math.sqrt);
// roots is now [1, 2, 3], numbers is still [1, 4, 9]
```

#### <code class="jsv-fs-inherit">pop</code>

The <code class="jsv-fs-inherit">pop()</code> method removes the last element from an array and returns that element.

```language-javascript
var alphabets = ['a', 'b', 'c', 'd'];
console.log(alphabets); // ['a', 'b', 'c', 'd']

var popped = alphabets.pop();
console.log(alphabets); // ['a', 'b', 'c' ] 
console.log(popped); // 'd'
```

#### <code class="jsv-fs-inherit">push</code>

The <code class="jsv-fs-inherit">push()</code> method adds one or more elements to the end of an array and returns the new length of the array.

```language-javascript
var sports = ['soccer', 'baseball'];
var total = sports.push('football', 'swimming');

console.log(sports); // ['soccer', 'baseball', 'football', 'swimming']
console.log(total);  // 4
```

#### <code class="jsv-fs-inherit">reduce</code>

The <code class="jsv-fs-inherit">reduce()</code> method applies a function against an accumulator and each value of the array (from left-to-right) has to reduce it to a single value. This method is extremely useful when you want to aggregate data.

```language-javascript
function add(x, y) {
  return x + y;
}
var total = [0, 1, 2, 3, 4].reduce(add);
// total == 10
```

* <code class="jsv-fs-inherit">reduceRight()</code>
* <code class="jsv-fs-inherit">reverse()</code>
* <code class="jsv-fs-inherit">shift()</code>
* <code class="jsv-fs-inherit">slice()</code>
* <code class="jsv-fs-inherit">some()</code>
* <code class="jsv-fs-inherit">sort()</code>
* <code class="jsv-fs-inherit">splice()</code>
* <code class="jsv-fs-inherit">unshift()</code>



