---
layout: post
title:  "JavaScript Objects in Detail"
date:   2015-11-15
categories: javascript
---

![JavaScript Objects in Detail](/images/js_object.png)

JavaScript’s core—most often used and most fundamental—data type is the Object data type. JavaScript has one complex data type, the Object data type, and it has five simple data types: Number, String, Boolean, Undefined, and Null. Note that these simple (primitive) data types are immutable (cannot be changed), while objects are mutable (can be changed).

__1. What is a object__

An object is an unordered list of primitive data types (and sometimes reference data types) that is stored as a series of name-value pairs. Each item in the list is called a property (functions are called methods).

Consider this simple object:

{% highlight javascript %}
var myFirstObject = {firstName: "Richard", favoriteAuthor: "Conrad"};
{% endhighlight %}

Think of an object as a list that contains items, and each item (a property or a method) in the list is stored by a name-value pair. The property names in the example above are firstName and favoriteAuthor. And the values are “Richard” and “Conrad.”

Property names can be a string or a number, but if the property name is a number, it has to be accessed with the bracket notation. More on bracket notation later. Here is another example of objects with numbers as the property name: 

{% highlight javascript %}
var ageGroup = {30: "Children", 100:"Very Old"};
console.log(ageGroup.30) // This will throw an error​
​// This is how you will access the value of the property 30, to get value "Children"​
console.log(ageGroup["30"]); // Children​
​
​//It is best to avoid using numbers as property names.
{% endhighlight %}

As a JavaScript developer you will most often use the object data type, mostly for storing data and for creating your own custom methods and functions.

__2. Reference Data Type and Primitive Data Types__

One of the main differences between reference data type and primitive data types is reference data type’s value is stored as a reference, it is not stored directly on the variable, as a value, as the primitive data types are. For example:

{% highlight javascript %}
// The primitive data type String is stored as a value​
​var person = "Kobe";
​var anotherPerson = person; // anotherPerson = the value of person​
person = "Bryant"; // value of person changed​
​
console.log(anotherPerson); // Kobe​
console.log(person); // Bryant
{% endhighlight %}

It is worth noting that even though we changed person to “Bryant,” the anotherPerson variable still retains the value that person had.

Compare the primitive data saved-as-value demonstrated above with the save-as-reference for objects:

{% highlight javascript %}
var person = {name: "Kobe"};
​var anotherPerson = person;
person.name = "Bryant";
​
console.log(anotherPerson.name); // Bryant​
console.log(person.name); // Bryant
{% endhighlight %}

In this example, we copied the person object to anotherPerson, but because the value in person was stored as a reference and not an actual value, when we changed the person.name property to “Bryant” the anotherPerson reflected the change because it never stored an actual copy of it’s own value of the person’s properties, it only had a reference to it.

__3. Object Data Properties Have Attributes__

Each data property (object property that store data) has not only the name-value pair, but also 3 attributes (the three attributes are set to true by default):
— Configurable Attribute: Specifies whether the property can be deleted or changed.
— Enumerable: Specifies whether the property can be returned in a for/in loop.
— Writable: Specifies whether the property can be changed.

__4. Creating Objects__

 Object Literals

{% highlight javascript %}
// This is an empty object initialized using the object literal notation​
​var myBooks = {};
​
​// This is an object with 4 items, again using object literal​
​var mango = {
  color: "yellow",
  shape: "round",
  sweetness: 8,
​
  ​howSweetAmI: function () {
    console.log("Hmm Hmm Good");
  }
}
{% endhighlight %}

 Object Constructor

{% highlight javascript %}
var mango =  new Object ();
mango.color = "yellow";
mango.shape= "round";
mango.sweetness = 8;
​
mango.howSweetAmI = function () {
  console.log("Hmm Hmm Good");
}
{% endhighlight %}

__5. How to Access Properties on an Object__

The two primary ways of accessing properties of an object are with dot notation and bracket notation.

 Dot Notation

{% highlight javascript %}
// We have been using dot notation so far in the examples above, here is another example again:​
​var book = {title: "Ways to Go", pages: 280, bookMark1:"Page 20"};
​
​// To access the properties of the book object with dot notation, you do this:​
console.log ( book.title); // Ways to Go​
console.log ( book.pages); // 280
{% endhighlight %}

 Bracket Notation

{% highlight javascript %}
/ To access the properties of the book object with bracket notation, you do this:​
console.log ( book["title"]); //Ways to Go​
console.log ( book["pages"]); // 280​
​
​//Or, in case you have the property name in a variable:​
​var bookTitle = "title";
console.log ( book[bookTitle]); // Ways to Go​
console.log (book["bookMark" + 1]); // Page 20 
{% endhighlight %}

Accessing a property on an object that does not exist will result in ```undefined```.

__6. Own and Inherited Properties__

Objects have inherited properties and own properties. The own properties are properties that were defined on the object, while the inherited properties were inherited from the object’s Prototype object.

To find out if a property exists on an object (either as an inherited or an own property), you use the in operator:

{% highlight javascript %}
// Create a new school object with a property name schoolName​
​var school = {schoolName:"MIT"};
​
​// Prints true because schoolName is an own property on the school object​
console.log("schoolName" in school);  // true​
​
​// Prints false because we did not define a schoolType property on the school object, and neither did the object inherit a schoolType property from its prototype object Object.prototype.​
console.log("schoolType" in school);  // false​

​// Prints true because the school object inherited the toString method from Object.prototype. ​
console.log("toString" in school);  // true

{% endhighlight %}

__7. hasOwnProperty__

To find out if an object has a specific property as one of its own property, you use the hasOwnProperty method. This method is very useful because from time to time you need to enumerate an object and you want only the own properties, not the inherited ones.

{% highlight javascript %}
// Create a new school object with a property name schoolName​
​var school = {schoolName:"MIT"};
​
​// Prints true because schoolName is an own property on the school object​
console.log(school.hasOwnProperty ("schoolName"));  // true​

​// Prints false because the school object inherited the toString method from Object.prototype, therefore toString is not an own property of the school object.​
console.log(school.hasOwnProperty ("toString"));  // false

{% endhighlight %}

To be continue [javascriptissexy](http://javascriptissexy.com/javascript-objects-in-detail/)