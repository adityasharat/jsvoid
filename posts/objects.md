I'm writing this post because a few days back I was trying to explain to a non-programmer: **what is an object**. Though it was just a conversation, nothing related to JavaScript or any language.
So, I thought why not write a post that would allow a non-programmer or JS beginner to understand what `objects` are.

# Objects in JavaScript?

JavaScript has a very simple concept of objects. An object is a set of properties, and a property is an association between a name and a value. A property's value can be of any type, when it is a `function` the property is known as a method. Lets learn how to use objects and their properties in JavaScript.

Objects in JavaScript, like in many other programming languages, can be compared to objects in real life.

In JavaScript, an object is a standalone entity, with properties associated with it. For example a car, has a make, model, color, seating capacity, price etc. The same way, JavaScript objects can have properties, which define their characteristics.

## Properties

Consider a property of an object as a variable that is attached to that object. Object properties are basically the same as ordinary JavaScript variables, except for the attachment to objects. You access the properties of an object with a simple dot-notation:

```language-javascript
nameOfObject.nameOfProperty
```

Names of *properties* are case sensitive. You can define a property by assigning it a value. For example, let's create an object named cuboid and give it properties length, width, height:

```language-javascript
var cuboid = new Object(); // let me explain this later

cuboid.length = 20;
cuboid.width = 15;
cuboid.height = 10;
```

You can also use the bracket notation to access or set properties. Objects are sometimes called associative arrays, since each property is associated with a string value that can be used to access it.

```language-javascript
cuboid['length'] = 20;
```

This is specially useful when the name of the property you want to access in stored in a variable. (when the property name is not determined until runtime)

```language-javascript
var dontKnowWhatThisIs = 'width';
cuboid[dontKnowWhatThisIs];	// 15
```

An object property name can be any valid JavaScript string, or anything that can be converted to a string, including the empty string. However, any property name that is not a valid JavaScript identifier (for example, a property name that has a space or a hyphen, or that starts with a number) can only be accessed using the square bracket notation.

```language-javascript
// works :)
cuboid['date created'] = '2015-04-28';

// works :)
cuboid[''] = '???';

// this will throw an error
cuboid.date-created = '2015-04-28';
// ReferenceError: created is not defined
```

Let me do something to show off how you can use what we just read.

```language-javascript
function showObjectPropsAndValues(obj, name) {
  var result = "";
  for (var i in obj) {
    // we will cover this if statement later
    if (obj.hasOwnProperty(i)) {
        result += name + "." + i + " = " + obj[i] + "\n";
    }
  }
  return result;
}

// let's call it on the cuboid
showObjectPropsAndValues(cuboid, 'cuboid');
```

So, The function call will return a string:

```language-javascript
'cuboid.length = 20
cuboid.width = 15
cuboid.height = 10'
```

### Enumerating all properties of an object

* `for...in` loops

	This method traverses all enumerable properties of an object and its prototype chain. See [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)

* `Object.keys(o)`

	This method returns an array with all the own (not in the prototype chain) enumerable properties' names ("keys") of an object o. See [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)
    
* `Object.getOwnPropertyNames(o)`

	This method returns an array containing all own properties' names (enumerable or not) of an object o. See [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames)
    
## Creating new objects

Remember this

```language-javascript
var cuboid = new Object();
```

### Using a constructor function

This is one of the methods to create an empty object. It's called the **constructor function**. The Object function when used with `new` keyword returns a new empty object. Let's create a constructor function to create `Cuboid` objects. When I mean `Cuboid` objects it means to create objects which are **NOT** empty but contains 3 properties: `length`, `width`, `height`.

```language-javascript
function Cuboid(x, y, z) {
  this.length = x;
  this.width = y;
  this.height = z;
}
```

Now you can create an object called cuboid0 as follows:

```language-javascript
var cuboid0 = new Cuboid(10, 12, 8);
```

This creates a Cuboid object and assigns it to the variable `cuboid0`.

```language-javascript
{
  length: 10,
  width: 12,
  height: 8
}
```

### Using Literal Notation 

You can create objects using an [object literal notation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer). Using object initializers is sometimes referred to as creating objects with literal notation.

Creating an empty object:

```language-javascript
var emptyObj = {};
```

Creating an cuboid object:

```language-javascript
var cuboid1 = {
  length: 10,
  width: 12,
  height: 8
};
```

Object initializers are expressions, and each object initializer results in a new object being created whenever the statement in which it appears is executed. Identical object initializers create distinct objects that will not compare to each other as equal. Objects are created as if a call to new Object() were made; that is, objects made from object literal expressions are instances of Object.

So remember:

```language-javascript
var cuboid2 = {
  length: 10,
  width: 12,
  height: 8
};

cuboid1 == cuboid2;
// false
```

Because they are two different objects, even if they have the same properties and values.

A property of an object and be another object.

```language-javascript
var cuboid3 = {
  length: 10,
  width: 12,
  height: 8,
  misc: {
    color: 'white',
    weight: 26
  }
};
```

### Using the `Object.create` method

I will describe this one when we do inheritance. :)

## Methods in Objects

A method is a function associated with an object, or, simply put, a method is a property of an object that is a function. Methods are defined the way normal functions are defined, except that they have to be assigned as the property of an object.

```language-javascript
objName.methodName = function (arguments) {
  // do something
};

// or
objName.methodName = functionName;

// or
var objName = {
  methodName = function (arguments) {
    // do something
  }
}
```

Here is an example:

```language-javascript
var cuboid2 = {
  length: 10,
  width: 12,
  height: 8
  whatAmI: function () {
    console.log('I\'m a cuboid');
  }
};

cuboid2.whatAmI();
// 'I'm a cuboid'
```

### Using `this` for object references

JavaScript has a special keyword, [this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this), that you can use within a method to refer to the current object. For example, suppose you have a function called volume that calculates the volume of the cuboid. For that it would need access to the `length`, `width` and `height` of the object.

```language-javascript
var cuboid2 = {
  length: 10,
  width: 12,
  height: 8
  volume: function () {
    return this.length * this.width * this.height;
  }
};

cuboid2.volume();
// 960
```

Since `volume` was called from cuboid2. For the `volume` function `this` refers to `cuboid2`. And `cuboid2.length` will return the length of the cube and so on.

There are a lot of cool stuff you can do with `this`. I will cover that, getters and setters, and Inheritance with another post.

Please leave your comments, suggestions and correction below. :)
