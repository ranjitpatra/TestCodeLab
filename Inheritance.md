# JavaScript Object inheritance

## Overview
This CodeLab introduces basic of Object inheritance.

### What you'll learn?

1. Basic overview

2. Object.create(proto) to create prototype.

3. Let's Isolate inheritance implementation to a common method.

4. How to call parent class properties from the child class ?

5. How to call superclass constructor?

6. How to implement multiple inheritance ?

## Basic overview

JavaScript is a class-free object-oriented language. it uses prototypical inheritance instead of classical inheritance. 
- In Classical inheritance : inheritance is achieved by defining classes of objects
- In Prototypical inheritance : Objects inherit from objects.

We can make prototype objects, and then we make new instances. As Objects are mutable in JavaScript, we can augment the new instances, giving them new fields and methods. These can then act as prototypes for even newer objects. We don't need classes to make lots of similar objects. This is how Objects inherit from objects.

## Object.create(proto) to create prototype.

Let think about we have two classes Mamal and Cat. Cat will inherit property from Mamal. Let's implement it.

```javascript
function Mammal(name){ 
	this.name = name;
	this.offspring = [];
} 

Mammal.prototype.haveABaby = function(){ 
	var newBaby = new Mammal("Baby "+this.name);
	this.offspring.push(newBaby);
	return newBaby;
} 
Mammal.prototype.toString=function(){ 
	return '[Mammal "'+this.name+'"]';
}


Cat.prototype = Object.create(Mamal.prototype);  // Here's where the inheritance occurs 
Cat.prototype.constructor = Cat;       			 // Otherwise instances of Cat would have a constructor of Mammal 
function Cat(name){ 
	this.name = name;
} 
Cat.prototype.toString = function(){ 
	return '[Cat "'+this.name+'"]';
}

var myPet = new Cat('Katty');
alert('myPet is '+  myPet);             // results in 'myPet is [Cat "Katty"]'

myPet.haveABaby();                    	// calls a method inherited from Mammal 
alert(myPet.offspring[0]);				// results in '[Mammal "Baby Katty"]' 
```

We have seen Object.create() will create a new instance of the prototype passed as argument. As we assigned to the Cat.prototype then it will override the constructor property. So again we have to set the constructor. 

The Object.create() is a part of ECMA-5 version, So it will not be available in older browser. The polyfill for the older browser is a temporary constructor, lets implement this.

```javascript
function inherit(proto) {
	function F() {}
	F.prototype = proto
	return new F
}
Cat.prototype = inherit(Mamal.prototype); // Here the inheritance will occurs.
```

Following are the steps for the inheritance : 

1. Inherit using ChildClass.prototype = new ParentClass();
2. Reset the constructor property for the child class using ChildClass.prototype.constructor = ChildClass.

## How to call parent class properties from the child class ?

In classical Object oriented model usually we have a special syntax to access superclass properties. The main reason of it we will add out common logic in the parent. Where child will do the same as the parent with some additional functionality. i.e. child will call the parent method with the same name as the child and adds the extra statements in its own method.

In JavaScript there is no such keywords to call parents but we can add some functionality like “superclass” to call the superclass methods. Following are the code snippets.

```javascript
Cat.prototype = Object.create(Mamal.prototype);  // Here's where the inheritance occurs 
Cat.prototype.constructor = Cat;       			 // Otherwise instances of Cat would have a constructor of Mammal
Cat.superclass = Mamal.prototype;				 // We are keeping the reference of parents prototype in the child


//The code snippet for calling superclass haveABaby()
Cat.prototype.haveABaby = function(){ 
	this.superclass.haveABaby.call(this);
}
```

## Let's Isolate inheritance implementation to a common method.

For inheritance as we saw we have some common codes like Prototype assignment, updating the constructor, and the use of temporary constructor. All the implementations can be done by a generic method and it will hold the implementation of the inheritance. let’s implement the method.

```javascript
function extent(Child, Parent){
    var F = function() {};
    F.prototype = Parent.prototype;
    Child.prototype = new F();
    Child.prototype.constructor = Child;
    //We are keeping the reference of parents prototype in the child
    Child.superclass = Parent.prototype;
}

extend(Cat, Mamal); // Now Cat inherits property from the parent Mamal
```

## How to call superclass constructor?

When a class in being instantiated, it first call the constructor and the invocation of constructor will chain upto the base class. But what we have implemented till now will not invoke the superclass constructor as we have a mechanism to invoke superclass method. The invocation of constructor is little bit different. Let’s write the code to invoke the superclass constructor.

```javascript
...

function Cat(name){ 
	this.name = name;
	this.constructor.superclass.constructor.apply(this, arguments); //This will call the superclass constructor.
}
...
```

## How to implement multiple inheritance ? 

By the previous approach we can not target the multiple inheritance as when we extend one class from other it will completely overwrite the prototype so the recent parent will take the advantage and will involve in inheritance. To achieve this we have to slightly change our implementation. what is this ?

The multiple inheritance can be achieved through the mixin. so what is mixin ? In mixin child will copy the prototype properties of the parent without overwriting it’s prototype. Means it will copy all the properties from the parent prototype to child prototype. Let’s implement the mixin;

```javascript
function mixin(){
    var args = Array,prototype.slice.call(arguments), Child, Parent;
    Child = args[0];
    args = args.slice(1);

    for(var i = 0, ln = args.length; i < ln; i++){
        Parent = args[i];
        for(var key in Parent.prototype){
            Child.prototype[key] = Parent.prototype[key];
        }
    }

}

mixin(Cat, Mamal, SomeOthetClass); 
```
