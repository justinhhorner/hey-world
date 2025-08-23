I listened to a [6-hour interview with DHH](https://www.youtube.com/watch?v=vagyIcmIGOQ&t=3324s), and one of the interesting parts to me as it relates to Ruby is this quote:

> Matz went the complete opposite direction. He believes in humanity. He believes in the unlimited capacity of programmers to learn and become better so much so that he’s willing to put the stranger at his own level. This is the second part I truly appreciate about Ruby. Ruby allows you to extend base classes. - DHH


Here's an example of reopening the Book class from Programming Ruby. We'll start with just a title attribute.

```
class Book
  attr_accessor :title
end
```


Later, we can reopen by declaring it again, this time with an additional method.

```
class Book
  def uppercase_title
    title.upcase
  end
end
```


Where in most languages this would be an error, in Ruby, the new definitions in the second declaration are added to the existing class! We now have access to uppercase_title on any book instance. This would still be useful even if it could only be done with your own classes, but this can be done for existing classes in Ruby itself.

As you'd expect, this capability is used extensively in Ruby on Rails. One example from the book is a method in Rails called squish that clears excessive whitespace in a string. It achieves this by reopening the String class to add the method, which can then be used on any string like a built-in method.

```
class String
  def squish
    # ...
  end
end

"Squish     this     string".squish
```

From a language design perspective, I love that Ruby makes it so easy to reopen classes in this way. However, some will certainly not want this for one reason or another. The general fear is reopening and changing the behavior of existing methods instead of the addition of new methods like squish. I've never had the desire to change an existing method. Even if I wanted to, I would probably add a new method instead.

There's still a lot remaining for me to get through in Programming Ruby, so I'm sure I'll find more interesting things to write about as I continue.