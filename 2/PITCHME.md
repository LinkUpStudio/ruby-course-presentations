# Introduction to @css[ruby-red no-text-transform](Ruby)
### @css[no-text-transform](pt. 2)

---

#### Conditionals

if / unless

```ruby
if 4.0 == 4
  'yay!'
  '4.0 is equal to 4!'
end
# => "4.0 is equal to 4!"

if 4.0 != 4 then '4.0 is NOT equal to 4!' end
# => nil

'totally TRUE!' if 0.1 + 0.2 == 0.3
# => nil

'somethig is NOT right...' unless 0.1 + 0.2 == 0.3
# => "somethig is NOT right..."
```
@[1-5]()
@[7-8]()
@[10-11]()
@[13-14]()

+++

if, else, elsif

```ruby
answer =  if b.zero?
            'b is zero'
          else
            'b is not zero'
          end
# => "b is not zero"

a = 6
if a < 5
  "#{a} is less than 5"
elsif a > 5
  "#{a} is greater than 5"
else
  "#{a} equals 5"
end
# => "6 is greater than 5"

answer = b.zero? ? 'b is zero' : 'b is not zero'
# => "b is not zero"

b.zero? ? (a != 4 ? 'wtf' : 'omg' ) : (a.zero? ? ':(' : "Nooo")
# => "Nooo"
```
@[1-6]()
@[8-16]()
@[18-19]()
@[21-22]()

+++

#### Boolean operators

Boolean "AND"

```ruby
nil && 99
# => nil
false && 99
# => false

'' && 99
# => 99
0 && 99
# => 99
-1 && 99
# => 99
```
@[1-2]()
@[3-4]()
@[6-7]()
@[8-9]()
@[10-11]()

+++

Boolean "OR"

```ruby
nil || 99
# => 99
false || 99
# => 99

'' || 99
# => ""
0 || 99
# => 0
-1 || 99
# => -1
```
@[1-2]()
@[3-4]()
@[6-7]()
@[8-9]()
@[10-11]()

+++

#### @css[ruby-red](Low priority) Boolean operators
@css[ruby-red](Better don't play with them)

```ruby
a = nil or 99     # => 99
a                 # => nil

a = 99 and nil    # => nil
a                 # => 99
```
@[1-2]()
@[4-5]()

+++

#### Bonus

```ruby
b = 8
answer = b.zero? && 'b is zero' || 'b is not zero'
# => "b is not zero"

b ||= 10
# => 8

a = false
a ||= true
# => true
```
@[1-3]()
@[5-6]()
@[8-10]()

+++

#### Case, when

```ruby
a = 1
r = case
    when a < 5 then "#{a} less than 5"
    when a > 5 then "#{a} greater than 5"
    else "#{a} equals 5"
    end
# => "1 less than 5"
```

+++

#### Case-when with parameter

```ruby
a = 1
case a
when 0...5
  "#{a} is between 0 and 5 (but not 5)"
when 5
  "#{a} equals 5"
when 5...Float::INFINITY
  "#{a} greater than 5"
else
  "#{a} less than 0"
end
# => "1 is between 0 and 5 (but not 5)"
```

---

#### Loops

most primitive

```ruby
i = 0
loop do
  i += 1
  break if i >= 10
  next if i.even?
  print "#{i} "
end
# 1 3 5 7 9 => nil
```

+++

#### While

```ruby
i = 0
while i < 10
  print "#{i} " if i.even?
  i += 1
end
# 0 2 4 6 8 => nil
```

+++

#### "Do While"

@css[ruby-red](Matz)\* told us to use `loop` instead,
check [here](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-core/6745)
and [here](http://rosettacode.org/wiki/Loops/Do-while#Ruby)

```ruby
i = 11
begin
  puts "i is #{i}"
  i += 1
end while i < 10
# i is 11
# => nil
```

\*Matz - Yukihiro Matsumoto, creator of @css[ruby-red](Ruby)

+++

#### Until

```ruby
i = 1
until i > 5
  print "#{i} "
  i += 1
end
# 1 2 3 4 5 => nil
```

+++

#### For

```ruby
for i in 1..10
  print "#{i} "
end
# 1 2 3 4 5 6 7 8 9 10 => 1..10
i
# => 10
```

+++

#### Times & Upto

```ruby
10.times { |i| print "#{i} " }
# 0 1 2 3 4 5 6 7 8 9 => 10

5.times { print '- ' }
# - - - - - => 5

1.upto(10) { |i| print "#{i} " }
# 1 2 3 4 5 6 7 8 9 10 => 1
```

---

#### Methods

```ruby
def movie_listing(title, rank = 5)
  %("#{title}" has a rank of #{rank})
end
# => :movie_listing

movie_listing("Lord of the Rings", 10)
# => "\"Lord of the Rings\" has a rank of 10"
movie_listing("Hobbit", 8)
# => "\"Hobbit\" has a rank of 8"
movie_listing('Some mediocre movie')
# => "\"Some mediocre movie\" has a rank of 5"
```
@[2](% is preferred shorthand for %Q)

+++

More arguments with default values

```ruby
def spanish_name(first = 'Rick', middle = '', last = 'Sanchez')
  "#{first} #{middle} #{last}".gsub(/\s{2,}/, ' ')
end

spanish_name
# => "Rick Sanchez"
spanish_name('Jose', 'Maria')
# => "Jose Maria Sanchez"
spanish_name('Jose', 'Maria', 'Rodriguez')
# => "Jose Maria Rodriguez"
```

+++

Ruby 2 Keyword arguments

https://robots.thoughtbot.com/ruby-2-keyword-arguments

```ruby
def spanish_name(name:, surname:, mother: nil)
  "#{name} #{mother} #{surname}".gsub(/\s{2,}/, ' ')
end

spanish_name(surname: 'Sanchez', name: 'Rick')
# => "Rick Sanchez"
```

+++

Splat operator again!

```ruby
def var_args(first, *rest)
  "first = #{first}, rest = #{rest}"
end

var_args('one')
# => "first = one, rest = []"

var_args('one', 'two')
# => "first = one, rest = [\"two\"]"

var_args('one', 'two', 'three')
# => "first = one, rest = [\"two\", \"three\"]"
```

+++

Splat, once again

```ruby
def split_apart(first, *other, last)
  "first: #{first}, other: #{other}, last: #{last}"
end

split_apart(1, 2)
# => "first: 1, other: [], last: 2"
split_apart(1, 2, 3)
# => "first: 1, other: [2], last: 3"
split_apart(1, 2, 3, 4)
# => "first: 1, other: [2, 3], last: 4"

def split_apart(first, *, last)
  # only first and last are available here
  # crazy, right?
end
```
@[1-3]()

+++

#### Return values

```ruby
def number_is(n)
  if n > 0
    'positive'
  elsif n < 0
    'negative'
  else
    'zero'
  end
end

number_is(23)
# => "positive"
number_is(0)
# => "zero"
```

+++

... actually using `return`

```ruby
def number_is(n)
  return 'positive' if n > 0
  return 'negative' if n < 0
  'zero'
end
```
@[2,3](This technique is called "Guard clause")

much better :)

+++

#### Methods with blocks

```ruby
def double(thing)
  yield(thing * 2)
end

double(3) { |val| "I got #{val}" }
# => "I got 6"

double('tom') do |val|
  "Then I got #{val}"
end
# => "Then I got tomtom"
```

+++

Block check

```ruby
def try
  if block_given?
    yield
  else
    'no block'
  end
end

try { 'hello' }
# => "hello"
try do 'hello' end
# => "hello"
```

---

#### Closures

##### What’s a Closure?

A closure is basically a function that:

- Can be treated like a variable, i.e. assigned to another variable, passed as a
  method argument, etc.

- Remembers the values of all the variables that were in scope when the function
  was defined and is able to access these variables even if it is executed in a
  different scope.

Put differently, a closure is a first-class function that has lexical scope.

> Blocks, procs, lambdas, and methods available in Ruby are collectively called
closures.

+++

More methods with blocks (or procs?)

```ruby
def thrice
  yield
  yield
  yield
end

x = 0
thrice { x += 1 }   # => 3
x                   # => 3

def seven_times(&block)
  puts block.class
  block.call
  thrice(&block)
  thrice(&block)
end

seven_times { x += 10 }
# Proc
# => 73
```
@[1-9]()
@[11-20]()

---

#### Classes

##### Creating the class

```ruby
class User
end
```

##### Define ruby objects (instances)

```ruby
user = User.new
user.class # => User
user.is_a?(User) # => true
```

##### Initializing class instances

```ruby
class User
  def initialize(first_name, last_name, email)
    @first_name = first_name
    @last_name = last_name
    @email = email
  end
end

User.new('Clay', 'Jensen', 'clay.jensen@gmail.com')
```

+++

##### Defining a @css[ruby-red no-text-transform](#full_name) method

```ruby
class User
  def initialize(first_name, last_name, email)
    @first_name = first_name
    @last_name = last_name
    @email = email
  end

  def full_name
    "#{@first_name} #{@last_name}"
  end
end

user = User.new('Clay', 'Jensen', 'clay.jensen@gmail.com')
user.full_name # => "Clay Jensen"
```

+++

##### Defining "getter" and "setter" methods

```ruby
class User
  def initialize(first_name, last_name, email)
    @first_name = first_name
    @last_name = last_name
    @email = email
  end

  def last_name
    @email
  end

  def last_name=(value)
    @email = value
  end
end

user = User.new('Clay', 'Jensen', 'clay.jensen@gmail.com')
user.last_name # => "Jensen"
user.last_name = 'Kamman'
user.last_name # => "Kamman"

user.first_name          # => NoMethodError: undefined method `first_name'
user.first_name = 'John' # => NoMethodError: undefined method `first_name='
```
@[2-6]()
@[8-10]()
@[12-14]()
@[17-20]()
@[22-23]()

+++

##### "getter" and "setter" syntax sugar

```ruby
class User
  attr_accessor :first_name
  attr_writer :last_name
  attr_reader :email

  def initialize(first_name, last_name, email)
    @first_name = first_name
    @last_name = last_name
    @email = email
  end
end

user = User.new('Clay', 'Jensen', 'clay.jensen@gmail.com')

user.first_name = 'John'
user.first_name # => "John"

user.last_name = 'Kamman'
user.last_name # => NoMethodError: undefined method 'last_name'

user.email # => "clay.jensen@gmail.com"
user.email = 'john.kamman@gmail.com'
# => NoMethodError: undefined method 'email='
```
@[2-4]()
@[15-16]()
@[18-19]()
@[21-23]()

+++

##### Virtual attributes

```ruby
class Product
  attr_reader :name, :price_cents

  def initialize(name, price)
    @name = name
    self.price = price
  end

  def price
    (price_cents / 100.0).round(2)
  end

  def price=(value)
    @price_cents = (value.round(2) * 100).to_i
  end
end

product = Product.new('Book', 8.99)
product.price_cents         # => 899
product.price               # => 8.99

product.price = 0.1 + 0.2   #=> 0.30000000000000004
product.price               #=> 0.3
product.price_cents         #=> 30
```
@[2]()
@[4-7]()
@[9-11]()
@[13-15]()
@[18-20]()
@[22-24]()

+++

##### Defining operators

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def +(object)
    Product.new(
      "#{name}, #{object.name}", price + object.price
    )
  end
end

book = Product.new('Book', 8.99)
magazine = Product.new('Magazine', 7.6)
merged_product = book + magazine

merged_product.name     # => "Book, Magazine"
merged_product.price    # => 16.59
```
@[9-13]()
@[16-18]()
@[20-21]()

+++

##### Objects comparison

```ruby
product1 = Product.new('Book', 8.99)
# => #<Product:0x00555cc8164de0 @name="Book", @price=8.99>
product2 = Product.new('Book', 8.99)
# => #<Product:0x00555cc8149388 @name="Book", @price=8.99>
product1 == product2    # => false

class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def ==(object)
    return false unless object.is_a?(Product)
    name == object.name && price == object.price
  end
end

product1 == product2    # => true
```
@[1-5]()
@[15-18]()
@[21]()

+++

##### Class method

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def self.total_amount(*products)
    products.map(&:price).reduce(:+)
  end
end

book = Product.new('Book', 20)
magazine = Product.new('Magazine', 10)
Product.total_amount(book, magazine) # => 30.0

class Product
  class << self
    def total_amount(*products)
      #code
    end
  end
end
```
@[9-11]()
@[14-16]()
@[18-24]()

+++

##### Class variables

```ruby
class Product
  attr_reader :name, :price

  @@count = 0

  def initialize(name, price)
    @name = name
    @price = price
    @@count += 1
  end

  def self.total_count
    @@count
  end
end

5.times { Product.new('Book', 20) }
Product.total_count # => 5
```
@[4]()
@[9]()
@[12-14]()
@[17-18]()

+++

##### Class constants

```ruby
class Product
  STORE_NAME = 'Auchan'
end

Product::STORE_NAME       # => "Auchan"

Product::MIN_PRICE = 10
Product::MIN_PRICE        # => 10

Product::MIN_PRICE = 20
# warning: already initialized constant Product::MIN_PRICE
# => 20
Product::MIN_PRICE # => 20
```
@[2]()
@[5]()
@[7-8]()
@[10-13]()

---

### Enough for today
