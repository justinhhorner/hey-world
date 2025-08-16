I've had some time to read through to chapter 5 in Programming Ruby, and I've got a list of topics I want to write about, starting with classes in Ruby. There are a few details that I found quite interesting once I understood how Ruby works behind the scenes.

As mentioned in my last post, Ruby is object-oriented, so let's take a look at what it takes to create a class that will allow for creating objects that represent books that are in stock. Note all the examples I'm using are straight from the book.

```ruby
"Date","ISBN","Price"
"2013-04-12","978-1-9343561-0-4",39.45
"2013-04-13","978-1-9343561-6-6",45.67
"2013-04-14","978-1-9343560-7-4",36.95
class BookInStock
  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
end
```

# Initialize

As you'd expect, the syntax is elegant and there's no unnecessary noise. The initialize method is the constructor for the class, where we also have two parameters, **isnb** and **price**. When an object is created using the new method, such as **BookInStock.new** , memory is allocated for the object, and the initialize method is automatically called and arguments passed to it.

In Ruby, instance variables are prefixed with **@**. You can see in the example we assign the incoming argument values for isbn and price to instance variables since the arguments will be out of scope once initialize is finished. This is quite an elegant solution when you consider what we're used to in most C-based languages, where we often use another name for the instance member or add a prefix like **m_** or use **this.** I much prefer having a standard by the language.

This is what it would look like to create a few objects from this class. I'm using the **p** method here to get more debugging info. Using **puts** would only display the class name and the object's unique identifier.

```ruby
b1 = BookInStock.new("isbn1", 3)
p b1 # <BookInStock:0x0000000109690190 @isbn="isbn1", @price=3.0>

b2 = BookInStock.new("ibsn2", 3.14)
p b2 # <BookInStock:0x00000001082f2458 @isbn="isbn2", @price=3.14>

b3 = BookInStock.new("isbn3", "5.67")
p b3 # <BookInStock:0x000000010b0992f8 @isbn="isbn3", @price=5.67>
```

# Attributes

Instance variables are private to created objects. Naturally, once you have instance variables, you'll probably want to get and set them at some point. I would start by writing the typical getter and setter methods. In Ruby, the value of the last expression in a method becomes the return value, so there's no reason to explicitly use the return keyword, although you could if you prefer it.

```ruby
class BookInStock
  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
  
  def isbn
    @isbn
  end
  
  def price
    @price
  end
end
```

Although the syntax is already minimal, there's an even more concise way to create these getter methods using the **attr_reader** method that will create these attribute reader methods for us. We provide it with **Symbols** that reference the instance variable names. Doing this will create accessor methods isbn and price that are identical to the ones in the example above.

```ruby
class BookInStock
  attr_reader :isbn, :price
  
  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
end
```


If I want to set the price in other languages, I would typically use a property or create a method like setPrice. In Ruby, we create a method using the name of the instance variable followed by = like this.

```ruby
class BookInStock
  attr_reader :isbn, :price
  
  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
  
  def price=(new_price)
    @price = new_price
  end
end

# Setting the price
book = BookInStock.new("isbn1", 33.80)
puts book.price # 33.8
book.price = book.price * .75
puts book.price # 25.349999999999998
```

We can also make setters more concise using the **attr_accessor**, which creates read and write attribute methods. There's also **attr_write** to handle rare cases where you want write-only access. Putting it altogether, the class now has read-only access to isbn and read + write access to price.

```ruby
class BookInStock
  attr_reader :isbn
  attr_accessor :price
    
  def initialize(isbn, price)
    @isbn = isbn
    @price = Float(price)
  end
end
```


This is as clean as it could be to create a class. Easy to read, write, and understand at a glance.

I'll write a follow-up to continue talking about classes. There's still plenty to cover, like access control, using other classes, and something that makes Ruby really interesting: reopening classes! (also known as "monkey-patching").