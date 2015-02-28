Sometime back, when I thought I was a **JavaScript Ninja** or something, using all those cool tricks to make my code smaller,  look more readable and run more efficiently. Well I just Google'd "<a href="https://www.google.co.in/search?q=javascript+tricks" target="_blank">JavaScript Tricks</a>"and picked up the top result. After a week... **BOOM!** Came out scores of new bugs from nowhere. What went wrong?

There are so many awesome tricks and tips you can use with JS and most of them are really awesome. But you must be really careful when you use them. Here are some of the mistakes I made.

###### Using logical AND/ OR for conditions

```language-javascript
var foo = 10;
if (foo === 10) {
  bar();
}
// the above if statement be can written like this
foo === 10 &amp;&amp; bar();

if (foo !== 5) {
  bar();
}
// and this too
foo === 5 || bar();

```

This is all great, but what if `foo = 0;` is a valid value and you still want to call `bar`. Well it would fail because `0` is falsy value in JS. Consider the following before you use this trick.

```language-javascript
[10] === 10 // is false
[10] == 10 // is true
'10' == 10 // is true
'10' === 10 // is false
[] == 0 // is true
[] === 0 // is false
'' == false // is true but true == &quot;a&quot; is false
'' === false // is false
```

###### Using <code class="jsv-fs-inherit">typeof</code> to check type of a variables
<code class="jsv-fs-inherit">typeof</code> : is a JavaScript unary operator. It returns a string that represents the primitive type of a variable. The key word here is <strong>primitive</strong>. There are just 5 primitives in JS: <code class="jsv-fs-inherit">number</code>, <code class="jsv-fs-inherit">string</code>, <code class="jsv-fs-inherit">object</code>, <code class="jsv-fs-inherit">boolean</code> and <code class="jsv-fs-inherit">undefined</code>. Remember the following:

```language-javascript
typeof 1; // number
typeof 'abc'; // string
typeof {}; // object
typeof false; // boolean
typeof undefined; // undefined
// this is important
typeof []; // object
typeof null; // object
typeof new Number(10); // object
// to check if something null
var something = null;
something === null; // true
```

It is better to use <code class="jsv-fs-inherit">instanceof</code> for objects

```language-javascript
var something = [];
something instanceof Array; // true
something = new Date();
something instanceof Date; // true
// But there is a catch
something instanceof Object; // is also true
```

So when you are checking for types start with primitives (undefined, null, boolean, number, string) followed by specific object types (Array, Date, etc) and then do <code class="jsv-fs-inherit">instanceof</code> Object.

###### Comparing Numbers

This can be a real problem when you are comparing numbers after complex mathematical calculations. Let me give you an example.

```language-javascript
var num = 5/0; // will not throw an exception like Java does
// the value of num will be Infinity
num === Infinity; // true

num = 0/0;   // neither will this
// the value of num will NaN
num === NaN; // false ..? yes, it will return false
NaN == NaN;  // false

// but
Infinity === Infinity; // true
typeof Infinity; // 'number'
typeof NaN;      // 'number'

// so how to check if a number is NaN
num = NaN;
isNaN(num); // true
```

###### Dangers of <code class="jsv-fs-inherit">for..in</code> loops

You can loop over properties of an object using the <code  class="jsv-fs-inherit">for..in</code> like this.

```language-javascript
var object = {
  name: 'John',
  age: 23,
  number: '11-22-33333'
};

for (var name in object) {  
  console.log(name);
}
// output would be
// name
// age
// number
```

This is a very awesome trick, as long you don't change something in the <code class="jsv-fs-inherit">prototype</code> of the <code  class="jsv-fs-inherit">Object</code>. For Example:

```language-javascript
Object.prototype.toJSON = function () {
  return JSON.stringify(this);
};
```

Now if you run the same <code class="jsv-fs-inherit">for..in</code> loop again the output would change.

```language-javascript
name
age
number
toJSON
// 'toJSON' will also get printed
```

You can avoid this by wrapping the body of the loop in <code class="jsv-fs-inherit">hasOwnProperty</code> like this.

```language-javascript
var object = {
  name: 'John',
  age: 23,
  number: '11-22-33333'
};

for (var name in object) {  
  if (object.hasOwnProperty(name)) {
      console.log(name);
  }
}
// output would be
// name
// age
// number
```

I would love to add to this list. Let me know if you know more and correct me if I'm wrong anywhere. :)
