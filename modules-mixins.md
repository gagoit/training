# Modules

Modules are a way of grouping together methods, classes, and constants. Modules give you two major benefits.
* Modules provide a namespace and prevent name clashes.
* Modules implement the mixin facility.
Unlike classes, you cannot create objects based on modules nor can you subclass them
```ruby
# Module defined in a.rb file

module A
  A_V = 6868
  def a1
  end
  
  def self.a2 x
    A_V / x
  end
end
```

To use any defined module, it can simply load the module files using the Ruby require statement 
```ruby
require filename
```

Example:
```ruby
require "a"

y = A.a2(2)
```

# Mixins in Ruby

Does Ruby support multiple inheritance?
-> No, but Ruby provide another way to do it: **mixin**

Mixins give you a wonderfully controlled way of adding functionality to classes via `include ModuleName` or `extend ModuleName`

```ruby
module A
  def a1
    puts "A.a1"
  end

  def fun
    puts "A.fun"
  end

  def self.a2
    puts "A.a2"
  end
end

module B
  def b1
    puts "B.b1"
  end

  def fun
    puts "B.fun"
  end

  def self.b2
    puts "B.b2"
  end
end
```

Example 1:
```ruby
class Sample
  include A
  include B

  def s1
    puts "Sample.s1"
  end
end

samp = Sample.new
samp.a1 => A.a1
samp.a2 => NoMethodError: undefined method `a2' for #<Sample
samp.b1 => B.b1
samp.b2 => NoMethodError: undefined method `b2' for #<Sample
samp.fun => B.fun
samp.s1  => Sample.s1

Sample.a1 => NoMethodError: undefined method `a1' for Sample:Class
Sample.a2 => NoMethodError: undefined method `a2' for Sample:Class
Sample.b1 => NoMethodError: undefined method `b1' for Sample:Class
Sample.b2 => NoMethodError: undefined method `b2' for Sample:Class
Sample.fun => NoMethodError: undefined method `fun' for Sample:Class
```

Example 2:
```ruby
class Sample
  extend A
  extend B

  def s1
    puts "Sample.s1"
  end
end

samp = Sample.new
samp.a1 => NoMethodError: undefined method `a1' for #<Sample
samp.a2 => NoMethodError: undefined method `a2' for #<Sample
samp.b1 => NoMethodError: undefined method `b1' for #<Sample
samp.b2 => NoMethodError: undefined method `b2' for #<Sample
samp.fun => NoMethodError: undefined method `fun' for #<Sample
samp.s1 => Sample.s1

Sample.a1 => A.a1
Sample.a2 => NoMethodError: undefined method `a2' for Sample:Class
Sample.b1 => B.b1
Sample.b2 => NoMethodError: undefined method `b2' for Sample:Class
Sample.fun => B.fun
```

==> 
## include: make instance methods in module to be instance method in class
## extend: make instance methods in module to be class method in class

`class methods in module haven't been effected`

## How to use both of instance methods and class methods in module?
```ruby
module Utilities  
  def method_one  
    puts "Hello from an instance method"  
  end
  
  def self.included(base)  
    base.extend(ClassMethods)  
  end

  module ClassMethods  
    def method_two  
      puts "Hello from a class method"  
    end  
  end  
end
```

Example
```ruby
class Abc
  include Utilities
end

abc = Abc.new
abc.method_one

Abc.method_two
```

## How to make the instance methods of module to be instance methods of a specific instance of a class?
```ruby
class Xyz
end

xyz_1 = Xyz.new
xyz_2 = Xyz.new

# Expectation
xyz_1.method_one => "Hello from an instance method"
xyz_2.method_one => NoMethodError: undefined method `method_one' for #<Xyz
```

==> `xyz_1.extend Utilities`

## How to make the instance methods of module to be class methods of a class after that class is defined?
```ruby
class De
end

# Expectation
De.method_one => "Hello from an instance method"
```

==> `De.extend Utilities`

```
Use mixins to sharing the methods between the classes. Instead of repeat the code for some methods, 
we can group the common methods in module and include/extend that module in the classes we want.
```

https://repl.it/@BillPham/modules-mixins

