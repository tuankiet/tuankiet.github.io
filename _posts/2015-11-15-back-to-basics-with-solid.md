---
layout: post
title: "Back to Basics: SOLID fundamental principles"
date: 2015-11-15
categories: ruby
---

![SOLID fundamental principles](/images/solid.png)

SOLID là một thuật ngữ gồm 5 nguyên tắc căn bản về việc maintain code của chúng ta trong bất kỳ ngôn ngữ hay phương pháp lập trình nào.

SOLID là nền tảng để chúng ta có thể đánh giá chất lượng và kiến trúc của codebase của hệ thống.

SOLID là viết tắt của 5 nguyên tắc sau:

* S - Single Responsibility Principle (Trách nhiệm đơn)

* O - Open/Close Principle (Nguyên tắc mở và đóng)

* L - Liskov Substitution Principle (Nguyên tắc thay thế của Liskov)

* I - Interface Segregation Principle (Nguyên tắc tách riêng các giao diện interface)

* D - Dependency Inversion Principle (Nguyên tắc về sự phụ thuộc ngược)


__1. S - Single Responsibility Principle (Trách nhiệm đơn)__

The Single Responsibility Principle is the most abstract of the bunch. It helps keep classes and methods small and maintainable. In addition to keeping classes small and focused it also makes them easier to understand.

While we all agree that focusing on a single responsibility is important, it’s difficult to determine what a class’s responsibility is. Generally, it is said that anything that gives a class a reason to change can be viewed as a responsibility. By change I am talking about structural changes to the class itself (as in modifying the code in the class’s file, not the object’s in-memory state). Let’s look at an example of some code that isn’t following the principle:

{% highlight ruby %}
class DealProcessor
  def initialize(deals)
    @deals = deals
  end

  def process
    @deals.each do |deal|
      Commission.create(deal: deal, amount: calculate_commission)
      mark_deal_processed
    end
  end

  private

  def mark_deal_processed
    @deal.processed = true
    @deal.save!
  end

  def calculate_commission
    @deal.dollar_amount * 0.05
  end
end
{% endhighlight %}

In the above class we have a single command interface that processes commission payments for deals. At first glance the class seems simple enough, but let’s look at reasons we might want to change this class. Any change in how we calculate commissions would require a change to this class. We could introduce new commission rules or strategies that would cause our ```calculate_commission``` method to change. For instance, we might want to vary the percentage based on deal amount. Any change in the steps required to mark a deal as processed in the ```mark_deal_processed``` method would result in a change in the file as well. An example of this might be adding support for sending an email summary of a specific person’s commissions after marking a deal processed. The fact that we can identify multiple reasons to change signals a violation of the Single Responsibility Principle.

We can do a quick refactor and get our code in compliance with the Single Responsibility Principle. Let’s take a look:

{% highlight ruby %}
class DealProcessor
  def initialize(deals)
    @deals = deals
  end

  def process
    @deals.each do |deal|
      mark_deal_processed
      CommissionCalculator.new.create_commission(deal)
    end
  end

  private

  def mark_deal_processed
    @deal.processed = true
    @deal.save!
  end
end

class CommissionCalculator
  def create_commission(deal)
    Commission.create(deal: deal, amount: deal.dollar_amount * 0.05)
  end
end
{% endhighlight %}

We now have two smaller classes that handle the two specific tasks. We have our processor that is responsible for processing and our calculator that computes the amount and creates any data associated with our new commission.

__2. O - Open/Close Principle (Nguyên tắc mở và đóng)__

The Open/Closed Principle states that classes or methods should be open for extension, but closed for modification. This tells us we should strive for modular designs that make it possible for us to change the behavior of the system without making modifications to the classes themselves. This is generally achieved through the use of patterns such as the strategy pattern. Let’s look at an example of some code that is violating the Open/Closed Principle:

{% highlight ruby %}
class UsageFileParser
  def initialize(client, usage_file)
    @client = client
    @usage_file = usage_file
  end

  def parse
    case @client.usage_file_format
      when :xml
        parse_xml
      when :csv
        parse_csv
    end

    @client.last_parse = Time.now
    @client.save!
  end

  private

  def parse_xml
    # parse xml
  end

  def parse_csv
    # parse csv
  end
end
{% endhighlight %}

In the above example we can see that we’ll have to modify our file parser anytime we add a client that reports usage information to us in a different file format. This violates the Open/Closed Principle. Let’s take a look at how we might modify this code to make it open to extension:

{%highlight ruby%}
class UsageFileParser
  def initialize(client, parser)
    @client = client
    @parser = parser
  end

  def parse(usage_file)
    parser.parse(usage_file)
    @client.last_parse = Time.now
    @client.save!
  end
end

class XmlParser
  def parse(usage_file)
    # parse xml
  end
end

class CsvParser
  def parse(usage_file)
    # parse csv
  end
end
{%endhighlight%}

With this refactor we’ve made it possible to add new parsers without changing any code. Any additional behavior will only require the addition of a new handler. This makes our UsageFileParser reusable and in many cases will keep us in compliance with the Single Responsibility Principle as well by encouraging us to create smaller more focused classes.

__3. L - Liskov Substitution Principle (Nguyên tắc thay thế của Liskov)__

Liskov’s principle tends to be the most difficult to understand. The principle states that you should be able to replace any instances of a parent class with an instance of one of its children without creating any unexpected or incorrect behaviors.

Let’s look at a example of a Liskov violation. We’ll start with the classic example of the relationship between a rectangle and a square. Let’s take a look:

{%highlight ruby%}
class Rectangle
  def set_height(height)
    @height = height
  end

  def set_width(width)
    @width = width
  end
end

class Square < Rectangle
  def set_height(height)
    super(height)
    @width = height
  end

  def set_width(width)
    super(width)
    @height = width
  end
end
{%endhighlight%}

For our Square class to make sense we need to modify both height and width when we call either set_height or set_width. This is the classic example of a Liskov violation. The modification of the other instance method is an unexpected consequence. If we were taking advantage of polymorphism and iterating over a collection of Rectangle objects one of which happened to be a Square, calling either method will result in a surprise. An engineer writing code with an instance of the Rectangle class in mind would never expect calling set_height to modify the width of the object.

Another common instance of a Liskov violation is raising an exception for an overridden method in a child class. It’s also not uncommon to see methods overridden with modified method signatures causing branching on type in classes that depend on objects of the parent’s type. All of these either lead to unstable code or unnecessary and ugly branching.

__4. I - Interface Segregation Principle (Nguyên tắc tách riêng các interface)__

This principle is less relevant in dynamic languages. Since duck typing languages don’t require that types be specified in our code this principle can’t be violated.

That doesn’t mean we shouldn’t take a look at a potential violation in case we’re working with another language. The principle states that a client should not be forced to depend on methods that it does not use.

Let’s take a closer look at what this means in Swift. In Swift, we can use protocols to define types that will require concrete classes to conform to the structures they outline. This makes it possible to create classes and methods that require only the minimum API.

In this example we’ll create two classes Test and User. We’ll also reference a Question class that I will omit since it will not be necessary for the sake of this example. Our user will take tests, and tests can be scored and taken. Let’s take a look:

{%highlight ruby%}
class Test {
  leg questions: [Question]
  init(testQuestions: [Question]) {
    questions = testQuestions
  }

  func answerQuestion(questionNumber: Int, choice: Int) {
    questions[questionNumber].answer(choice)
  }

  func gradeQuestion(questionNumber: Int, correct: Bool) {
    question[questionNumber].grade(correct)
  }
}

class User {
  func takeTest(test: Test) {
    for question in test.questions {
      test.answerQuestion(question.number, arc4random(4))
    }
  }
}
{%endhighlight%}

Our User would not get a very good grade because they’re randomly choosing test answers, but we also have a violation of the Interface Segregation Principle. Our takeTest requires we provide an argument of type Test. The Test type has two methods. takeTest depends on answerQuestion but does not care about gradeQuestion. Let’s take advantage of Swift’s protocols to fix this and get back on the right side of our Interface Segregation Principle.

{%highlight ruby%}
protocol QuestionAnswering {
  var questions: [Question] { get }
  func answerQuestion(questionNumber: Int, choice: Int)
}

class Test: QuestionAnswering {
  let questions: [Question]
  init(testQuestions: [Question]) {
    self.questions = testQuestions
  }

  func answerQuestion(questionNumber: Int, choice: Int) {
    questions[questionNumber].answer(choice)
  }

  func gradeQuestion(questionNumber: Int, correct: Bool) {
    question[questionNumber].grade(correct)
  }
}

class User {
  func takeTest(test: QuestionAnswering) {
    for question in test.questions {
      test.answerQuestion(question.number, arc4random(4))
    }
  }
}
{%endhighlight%}

Our takeTest method now requires a QuestionAnswering type. This is an improvement because we can now use this same logic for any type that conforms to this protocol. Perhaps down the road we would like to add a Quiz type, or even different types of tests. With our new implementation we could easily reuse this code.


__5. D - Dependency Inversion Principle (Nguyên tắc về sự phụ thuộc ngược)__

The Dependency Inversion Principle has to do with high-level (think business logic) objects not depending on low-level (think database querying and IO) implementation details. This can be achieved with duck typing and the Dependency Inversion Principle. Often this pattern is used to achieve the Open/Closed Principle that we discussed above. In fact, we can even reuse that same example as a demonstration of this principle. Let’s take a look:

{%highlight ruby%}
class UsageFileParser
  def initialize(client, parser)
    @client = client
    @parser = parser
  end

  def parse(usage_file)
    parser.parse(usage_file)
    @client.last_parse = Time.now
    @client.save!
  end
end

class XmlParser
  def parse(usage_file)
    # parse xml
  end
end

class CsvParser
  def parse(usage_file)
    # parse csv
  end
end
{%endhighlight%}

As you can see, our high-level object, the file parser, does not depend directly on an implementation of a lower-level object, XML and CSV parsers. The only thing that is required for an object to be used by our high-level class is that it responds to the parse message. This decouples our high-level functionality from low-level implementation details and allows us to easily modify what those low-level implementation details are. Having to write a separate usage file parser per file type would require lots of unnecessary duplication.

Follow by [Back to Basics: SOLID](https://robots.thoughtbot.com/back-to-basics-solid)