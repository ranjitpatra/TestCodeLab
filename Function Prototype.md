# JavaScript Object prototype

## Overview
This CodeLab introduces basic of Object prototype.

### What you'll learn?

1. What is Object prototype?

2. How it works?

3. How can we create prototype?

4. Object.create() to create prototype

5. Constructor function to create prototype

6. Using the prototype property

7. Augmenting Built-in Objects using prototype

## What is Object prototype?

1. Prototype providing way to add attribute and behaviour to a instance and prototype also a object.

2. Objects created using an object literal, or with new Object(), inherit from a prototype called Object.prototype. Object is the top most superclass in the class hierarchy. So all the subclasses like Function, Date, Array, etc... will inherit property from the Object.prototype.

## How it works?

Every object in JavaScript has a prototype. When a message reaches an object, JavaScript will attempt to find in the object first, If not found then it will send to the objects prototype and so on. it will work like single parent (Multi level) inheritance in a class based language.

## How can we create prototype?

Javascript providing three way's to create prototype.
1. Object.create

2. Constructor function

3. Using the prototype property.

## Object.create() to create prototype

The Object.create() method creates a new object with the specified prototype object and properties. the signature is Object.create(prototype, [properties]);

```javascript
var person = {
  	kind: 'person'
}

// creates a new object which prototype is person
var tom = Object.create(person);
  
console.log(tom.kind); // => ‘person’
```

We can pass an object to Object.create to add specific properties for a new object.

```javascript
var tom = Object.create(person, {age: {value:  30} });
console.log(tom.age); // => ‘30’
```

We can add more properties with a better and managed way. to know more please refer "https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/Object/create";

## Constructor function to create prototype

In javascript (ECMA5) we can create a constructor, by instantiating a function with "new" keyword. lets create a person class and add some properties there.

```javascript
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.say = function(){
    	return "Hi I am " + this.firstName + " " + this.lastName;
    }
}

var tom = new Person("Tom", Singh);
tom.say(); // Hi I am Tom Singh
```

Here the firstName, lastName and say() binded to the instance object by using this keyword. so we can access the properties or methods from the instance.

The common misatake is we have added the method say() in side the constructor function. For each instance of the class Person, will create new function say() and bind to it i.e. if we create 100 instance of Person then 100 new say() will be created also it will create memory relared issues.

To avoid this we can assign the behaviour/method to a common/shared object called "prototype" and we can reuse it whenever required.

## Using the prototype property

Lets rewite the above code using both constructor function and prototype property.

```javascript
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
}

Person.prototype.say = function(){
	return "Hi I am " + this.firstName + " " + this.lastName;
}

var tom = new Person("Tom", "Singh");
tom.say(); // Hi I am Tom Singh
```

Person.prototype is a shared scope for all the instances of the class Person. So the function Person.prototype.say() will be created once and accessible with all the Person instances.

It is advisible to keep all the reusable methods/properties in the perototype property.

## Augmenting Built-in Objects using prototype

The built-in objects such as the constructor functions Array, String, and even Object, and Function can be augmented through their prototypes, which means that you can, i.e. you can add new methods to the Array prototype and in this way make them available to all arrays. Let’s do this.

let’s add a method to inArray() to the Array which tells us whether a value/object is present in a array or not.

```javascript
Array.prototype.inArray = function(value){
    return this.indexOf(value) === -1 ? false : true;
}
var array = ["orange", "apple", "banana"];
array.inArray("apple"); // will return ture
array.inArray("grape"); // will return false

```

While augmenting build-in type, make a check whether this functionality is present in the prototype or not. let's implement it.

```javascript
if(!Array.prototype.inArray){ // Checking whether inArray is available in Array.prototype or not.
	Array.prototype.inArray = function(value){
	    return this.indexOf(value) === -1 ? false : true;
	}
}
```
