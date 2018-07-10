# Introduction to @css[ruby-red no-text-transform](Ruby)
### @css[no-text-transform](pt. 3)

---

#### Method visibility

All methods are `public` by **default**.

You can also use `protected` and `private`.

+++

Protected

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def together_price(other)
    discount_price + other.discount_price
  end

  protected

  def discount_price
    (price * 0.9).round(2)
  end
end

book = Product.new('Book', 8)
book.discount_price                 # => NoMethodError

magazine = Product.new('Magazine', 5)
book.together_price(magazine)       # => 11.7
```
@[4-7]()
@[9-11]()
@[13-17]()
@[20-21]()
@[23-24]()

+++

Private

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def full_cost
    price + tax
  end

  private

  def tax
    @tax ||= (price * 0.2).round(2)
  end
end

book = Product.new('Book', 8)
book.tax                            # => NoMethodError
book.full_cost                      # => 9.6
```
@[9-11]()
@[13-17]()
@[20-22]()

+++

`private` is too private sometimes

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def non_working_full_cost
    price + self.tax
  end

  private

  def tax
    @tax ||= (price * 0.2).round(2)
  end
end

book = Product.new('Book', 8)
book.non_working_full_cost          # => NoMethodError
```
@[9-11]()
@[20-21]()


+++

but `private` is OK for **setters**

```ruby
class Product
  attr_reader :name, :price, :discount

  def initialize(name, price)
    @name = name
    @price = price
    self.discount = 0
  end

  def full_cost
    price + tax - discount
  end

  def make_discounted!
    self.discount = price / 3.0
    self
  end

  private

  def discount=(value)
    @discount = value.round(2)
  end

  def tax
    @tax ||= (price * 0.2).round(2)
  end
end

book = Product.new('Book', 8)
book.full_cost                    # => 9.6
book.make_discounted!.full_cost   # => 6.93
book.full_cost                    # => 6.93
```
@[2]()
@[4-8]()
@[10-12]()
@[14-17]()
@[19-23]()
@[30-33]()

---

#### Inheritance

Method inheritance

```ruby
class A
  protected

  def protected_method
    'protected method called'
  end

  private

  def private_method
    'private method called'
  end
end

class B < A
  def call_private_and_protected
    private_method + ' and ' + protected_method
  end
end

b = B.new
b.call_private_and_protected
# => "private method called and protected method called"
```
@[2-6]()
@[8-12]()
@[15-19]()
@[21-23]()

+++

Method `super`

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end
end

class Book < Product
  attr_reader :author

  def initialize(author, name, price)
    @author = author
    super(name, price)
  end
end

poodr = Book.new('Sandi Metz', 'POODR', 19.79)
poodr.author                    # => "Sandi Metz"
poodr.name                      # => "POODR"
poodr.price                     # => 19.79
```
@[1-8]()
@[10-17]()
@[19-22]()

+++

`super` without arguments

```ruby
class Book < Product
  attr_reader :author

  def initialize(author, name, price)
    @author = author
    super
  end
end

poodr = Book.new('Sandi Metz', 'POODR', 19.79)
# => ArgumentError
#   (wrong number of arguments (given 3, expected 2))
```
@[4-7](super with no args here)
@[10-12](given 3?)

+++

No `super`?

```ruby
class Book < Product
  attr_reader :author

  def initialize(author, name, price)
    @author = author
  end
end

poodr = Book.new('Sandi Metz', 'POODR', 19.79)
poodr.author                    # => "Sandi Metz"
poodr.name                      # => nil
poodr.price                     # => nil
```
@[4-6](total method override)
@[9-12]()

+++

##### Class variables inheritance

@css[ruby-red](total mess), **DO NOT** use them at all.

```ruby
class A
  @@value = 1
  def self.value
    @@value
  end
end
A.value           # => 1

class B < A
  @@value = 2
end

class C < A
  @@value = 3
end

B.value           # => 3
```
@[1-7]()
@[9-11]()
@[13-15]()
@[17]()

+++

Constants are good.

But please **don't change** them :)

```ruby
class A
  NUM = 2
end

class B < A
end

A::NUM        # => 2
B::NUM        # => 2

B::NUM = 3

A::NUM        # => 2
B::NUM        # => 3
```
@[1-3]()
@[5-6]()
@[8-9]()
@[11]()
@[13-14]()

---

#### Modules

As namespaces

```ruby
module AudioConverter
  class Decoder
    def initialize(file)
      @file = file
      @format = AudioHelpers.get_format(file)
    end
  end

  class Encoder
    def initialize(file)
      @file = file
      @format = AudioHelpers.get_format(file)
    end
  end

  module AudioHelpers
    def self.get_format(file)
      File.extname(file)
    end
  end
end

AudioConverter::Decoder.new('music.mp3')
# => #<AudioConverter::Decoder ...
#      @file="music.mp3", @format=".mp3">
```
@[1]()
@[2-7]()
@[9-14]()
@[16-20]()
@[23-25]()

+++

As mixins

```ruby
module FileHelpers
  def get_format(file)
    File.extname(file)
  end
end

module AudioConverter
  class Decoder
    include FileHelpers

    def initialize(file)
      @file = file
      @format = get_format(file)
    end
  end

  class Encoder
    include FileHelpers

    def initialize(file)
      @file = file
      @format = get_format(file)
    end
  end
end

AudioConverter::Decoder.new('music.mp3')
# => #<AudioConverter::Decoder ...
#      @file="music.mp3", @format=".mp3">
```
@[1-5]()
@[8-15]()
@[17-24]()
@[27-29]()

---

#### Yay! End for today!
