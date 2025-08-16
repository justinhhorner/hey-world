I'm now on chapter 6 in Programming Ruby, but before I leave the topic of classes I want to share a few more details I find interesting about classes, starting with access control.

**Access Control
**In Ruby, we have three levels of access for methods. You probably are well aware of these terms, but I'll define them for completeness anyway.

**Public
**Methods that can be called by anyone. This is the default access modifier for methods, with the exception of initialize as it's always private (Ruby class this when new is called to create an object).

**Protected
**Methods that can be called only by objects of the defining class (and its subclasses). These are methods that belong to classes throughout the inheritance chain.

**Private
**In Ruby, access control is determined dynamically. This means you will only know about access violations during code execution; there's no opportunity to discover these during a compile or interpreter step.

Now it's getting a bit more interesting. If access control is dynamic, that must mean we're not using Ruby keywords, but instead calling methods. The access modifiers themselves are just methods: public, protected, and private. 

Thanks to this, we have some flexibility as to how access is defined in a class. First, we can call the access methods with no arguments. This will set the default access control of subsequent method definitions.

```ruby
class MyClass
  # Make the default "public"
  def method1
    # This method is public
  end
  
  protected
  # subsequent methods will be "protected"
  def method2
    # This method is protected
  end
  
  private
  # subsequent methods will be public
  def method3
    # This method is private
  end
end
```


You might think the lack of indentation for defined methods is a bit strange, but remember that the access methods are not creating a new block; only changing the access of subsequent methods.

We can make it even more clear that access control is handled by methods by writing it this way.

```ruby
class MyClass
  def method1
  end
  
  def method2
  end
  
  def method3
  end
  
  def method4
  end
  
  public :method1, :method4
  protected :method2
  private :method3
end
```


Apparently, this approach is quite rare in practice; however, it demonstrates well how access control is handled by methods that take symbols as arguments. How does this work, though? What is a symbol, and how do we have access to them?

If you're familiar with the concept of symbols from another language, know that it's probably the same in Ruby. Symbols are often used as an immutable identifier to access data. For example, you want a unique, immutable value to use as a key in a hash, and in this case, the name of a method. In the example above you can see symbols are prefixed with a colon.

Most statements in Ruby return a value, and that's what's happening here with methods. Defined methods return the name of the method as a symbol that we can provide to access control methods as identifiers for that access control to be set at runtime.

With all that said, if def returns a symbol for the method, and access control methods take symbols as arguments, then that would lead us to the most common form you'll see in classes.

```ruby
class MyClass
  def method1
    # This method is public
  end
  
  protected def method2
    # This method is protected
  end
  
  private def method3
    # This method is private
  end
  
  public def method4
    # This method is public
  end
end
```


I hope to find more time to read through the book in the coming weeks. The next topic I want to write about is reopening classes.