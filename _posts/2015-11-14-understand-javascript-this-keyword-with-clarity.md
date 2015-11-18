---
layout: post
title:  "Understand \"This\" in JS with clarity"
date:   2015-11-14
categories: javascript
---

![Understand \"This\" in JS with clarity](/images/understand-js-this.jpg)

Trong Javascript, thuật ngữ `This` đưọc sử dụng khá phổ biến, và không phải ai cũng có thể hiểu thật sự rõ vế nó. `This` thường gây nhiều nhầm lẫn cho developer. Mục đích bài viết này là phân tích nắm rõ về `This` và giúp developer yên tâm, không bị lo lắng khi lập trình với `This`. Làm thế nào sử dụng nó đúng ngữ cảnh, đúng trường hợp?

__1. Introduce `This`__

Với đoạn code sau:

{% highlight javascript %}
var person = {
    firstName: "Peter",
    lastName: "Pan",
    fullName: function () {
      // Notice we use "this" just as referer to person object:
        console.log(this.firstName + " " + this.lastName);
    ​  // We could have also written this:​
        console.log(person.firstName + " " + person.lastName);
    }
}
{% endhighlight %}

Nếu sử dụng ```person.firstName``` và ```person.lastName``` thì đoạn code trong ```function()``` không đưọc minh bạch và khó debug nếu như ```person``` là một biến global. Nói thêm ở đây ```firstName``` & ```lastName``` là các ```property``` của ```person```.
Chúng ta sử dụng ```this``` không chỉ mang lại tính thẩm mỹ cho code, tính tham khảo ngữ cảnh mà còn mang lại tính chính xác. ```This``` nói với chúng ta cái mà nó đang tham khảo đến.

__2. Basic of use `This`__

```First, know that all functions in JavaScript have properties, just as objects have properties. And when a function executes, it gets the this property—a variable with the value of the object that invokes the function where this is used.```

Ta biết rằng tất cả functions trong JS đều có những thuộc tính ```properties```, tựa như ```objects```. Và khi functions thực thi, nó sẽ gọi thuộc tính đó ra (thuộc tính là 1 biến có chứa value của object mà object đó đang được gọi ```invokes``` trong function đó, tức là theo ngữ cãnh ```context```)

```The this reference ALWAYS refers to (and holds the value of) an object—a singular object—and it is usually used inside a function or a method, although it can be used outside a function in the global scope. Note that when we use strict mode, this holds the value of undefined in global functions and in anonymous functions that are not bound to any object.```

```This``` thì luôn tham khảo và đồng thời nắm giữ value của 1 object và nó thường được dùng bên trong 1 function hoặc 1 method, mặc dù nó cũng có thể đưọc dùng bên ngoài function (trong global scope của JS). Ghi chú, khi ta dùng 1 cách nghiêm ngặt thì ```this``` chứa đựng giá trị ```undefined``` trong global functions và trong anomyous functions, nơi mà không là phạm vi của bất kỳ object nào.

```this is used inside a function (let’s say function A) and it contains the value of the object that invokes function A. We need this to access methods and properties of the object that invokes function A, especially since we don’t always know the name of the invoking object, and sometimes there is no name to use to refer to the invoking object. Indeed, this is really just a shortcut reference for the “antecedent object”—the invoking object.```

Ví dụ ta có function A, và ```this``` được dùng bên trong function A. Nó chứa đựng giá trị của object mà đưọc gọi trong function A.
Ta cần gọi hàm và thuộc tính của object đó, chú ý là ta không hề biết tên của những thứ đó, và đôi khi object đó không có bất kì để mà gọi. ```This``` thật sự nó là 1 ```shortcut``` tham khảo cho ```antecedent object```- ```the invoking object```.

Nhìn lại ví dụ cơ bản về ```this```:

{% highlight javascript %}
var person = {
    firstName   :"Penelope",
    lastName    :"Barrymore",
    // Since the "this" keyword is used inside the showFullName method below, and the showFullName method is defined on the person object,
    // "this" will have the value of the person object because the person object will invoke showFullName ()
    showFullName:function () {
    console.log (this.firstName + " " + this.lastName);
    }
  }

  person.showFullName (); // Penelope Barymore
{% endhighlight %}

Và so sánh với ```this``` trong ```JQuery```:

{% highlight javascript %}
// A very common piece of jQuery code
$ ("button").click (function (event) {
  // $(this) will have the value of the button ($("button")) object
  // because the button object invokes the click () method
    console.log ($ (this).prop ("name"));
});
{% endhighlight %}

jQuery dùng ```$(this)``` = Javascript thì dùng ```this```
```$(this)```  sử dụng bên trong ```anomyous function``` và function này thực thi trong ```button's click() method```.
Lý do ```$(this)``` ràng buộc trong object button bởi vì jQuery binds ```$(this)``` đến object mà nó đưọc gọi trong click method. Do đó, ```$(this)``` chứa giá trị của jQuery button object, mặc dù ```$(this)``` được định nghĩa bên trong ```anomyous function```.

Chú ý là button object là ```DOM element``` trên HTML page.

__3. Use ```This``` in global scope__

{% highlight javascript %}
var firstName = "Peter", lastName = "Pan";

function showFullName () {
  // "this" inside this function will have the value of the window object
  // because the showFullName () function is defined in the global scope, just like the firstName and lastName
  console.log (this.firstName + " " + this.lastName);
}

var person = {
  firstName   :"Penelope",
  lastName    :"Barrymore",
  showFullName:function () {
  // "this" on the line below refers to the person object, because the showFullName function will be invoked by person object.
  console.log (this.firstName + " " + this.lastName);
  }
}

showFullName (); // Peter Pan

// window is the object that all global variables and functions are defined on, hence:
window.showFullName (); // Peter Pan

// "this" inside the showFullName () method that is defined inside the person object still refers to the person object, hence:
person.showFullName (); // Penelope Barrymore

{% endhighlight %}

To be continue [javascriptissexy](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/)