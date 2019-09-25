# Avoid 'God' Objects

Classes should not violate the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), and the worst example of this violation can be the "God Object" - a class that knows about almost everything.

In many systems, including our own, the `User` class can become a magnet for behaviour. A big `User` class is not _necessarily_ bad - users are of course a very important component of the system, and should be respected as such. But this understanding should not lead to complacency, and 

## Bad

````ruby
class User
  def first_name
    @first_name
  end
  
  def last_name
    @last_name
  end
  
  def last_initial
    last_name[0...1]
  end
  
  def full_name
    "#{first_name} #{last_name}"
  end
  
  def display_name
    return "#{first_name} #{last_initial}." if hide_last_name?
    
    full_name
  end
  
  def to_s
    full_name
  end
end
````

## Good

````ruby
class User
  def name
    Name.new(self)
  end
end

class Name
  def initialize(user)
    @user = user
  end

  def first_name
    @user.first_name
  end
  
  def last_name
    @user.last_name
  end
  
  def last_initial
    last_name[0...1]
  end
  
  def full_name
    "#{first_name} #{last_name}"
  end
  
  def display_name
    return "#{first_name} #{last_initial}." if user.hide_last_name?
    
    full_name
  end
  
  def to_s
    full_name
  end
end
````
