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
#=> "4.0 is equal to 4!"

if 4.0 != 4 then '4.0 is NOT equal to 4!' end
#=> nil

'totally TRUE!' if 0.1 + 0.2 == 0.3
#=> nil

'somethig is NOT right...' unless 0.1 + 0.2 == 0.3
#=> "somethig is NOT right..."
```
<!-- @[1-5]() -->

+++

if, else, elsif

```ruby
answer =  if b.zero?
            'b is zero'
          else
            'b is not zero'
          end
#=> "b is not zero"

a = 6
if a < 5
  "#{a} is less than 5"
elsif a > 5
  "#{a} is greater than 5"
else
  "#{a} equals 5"
end
#=> "6 is greater than 5"

answer = b.zero? ? 'b is zero' : 'b is not zero'
#=> "b is not zero"

b.zero? ? (a != 4 ? 'wtf' : 'omg' ) : (a.zero? ? ':(' : "Nooo")
#=> "Nooo"
```

+++

#### Boolean operators

Boolean "AND"

```ruby
nil && 99
#=> nil
false && 99
#=> false

'' && 99
#=> 99
0 && 99
#=> 99
-1 && 99
#=> 99
```

+++

Boolean "OR"

```ruby
nil || 99
#=> 99
false || 99
#=> 99

'' || 99
#=> ""
0 || 99
#=> 0
-1 || 99
#=> -1
```

+++

#### @css[ruby-red](Low priority) Boolean operators
@css[ruby-red](Better don't play with them)

```ruby
a = nil or 99     #=> 99
a                 #=> nil

a = 99 and nil    #=> nil
a                 #=> 99
```

+++

#### Bonus

```ruby
b = 8
answer = b.zero? && 'b is zero' || 'b is not zero'
#=> "b is not zero"

b ||= 10
#=> 8

a = false
a ||= true
#=> true
```

+++

#### Case, when

```ruby
a = 1
r = case
    when a < 5 then "#{a} less than 5"
    when a > 5 then "#{a} greater than 5"
    else "#{a} equals 5"
    end
#=> "1 less than 5"
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
#=> "1 is between 0 and 5 (but not 5)"
```

---

#### Loops

most primitive, (@css[ruby-red](Matz)\* told us not to use it)

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

\*Matz - Yukihiro Matsumoto, creator of @css[ruby-red](Ruby)

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

```ruby
i = 11
begin
  puts "i is #{i}"
  i += 1
end while i < 10
# i is 11
#=> nil
```

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
#=> 10
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

#### Methods & friends

```ruby
def movie_listing(title, rank = 5)
  %("#{title}" has a rank of #{rank})
end
#=> :movie_listing

movie_listing("Lord of the Rings", 10)
#=> "\"Lord of the Rings\" has a rank of 10"
movie_listing("Hobbit", 8)
#=> "\"Hobbit\" has a rank of 8"
movie_listing('Some mediocre movie')
#=> "\"Some mediocre movie\" has a rank of 5"
```
@[2](% is preferred shorthand for %Q)

+++

More arguments with default values

```ruby
def spanish_name(first = 'Rick', middle = '', last = 'Sanchez')
  "#{first} #{middle} #{last}".gsub(/\s{2,}/, ' ')
end

spanish_name
#=> "Rick Sanchez"
spanish_name('Jose', 'Maria')
#=> "Jose Maria Sanchez"
spanish_name('Jose', 'Maria', 'Rodriguez')
#=> "Jose Maria Rodriguez"
```

+++

Ruby 2 Keyword arguments

https://robots.thoughtbot.com/ruby-2-keyword-arguments

```ruby
def spanish_name(name:, surname:, mother: nil)
  "#{name} #{mother} #{surname}".gsub(/\s{2,}/, ' ')
end

spanish_name(surname: 'Sanchez', name: 'Rick')
#=> "Rick Sanchez"
```

+++

Splat operator again!

```ruby
def var_args(first, *rest)
  "first = #{first}, rest = #{rest}"
end

var_args('one')
#=> "first = one, rest = []"

var_args('one', 'two')
#=> "first = one, rest = [\"two\"]"

var_args('one', 'two', 'three')
#=> "first = one, rest = [\"two\", \"three\"]"
```

+++

Splat, once again

```ruby
def split_apart(first, *other, last)
  "first: #{first}, other: #{other}, last: #{last}"
end

split_apart(1, 2)
#=> "first: 1, other: [], last: 2"
split_apart(1, 2, 3)
#=> "first: 1, other: [2], last: 3"
split_apart(1, 2, 3, 4)
#=> "first: 1, other: [2, 3], last: 4"

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
#=> "positive"
number_is(0)
#=> "zero"
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
#=> "I got 6"

double('tom') do |val|
  "Then I got #{val}"
end
#=> "Then I got tomtom"
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
#=> "hello"
try do 'hello' end
#=> "hello"
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
thrice { x += 1 }   #=> 3
x                   #=> 3

def seven_times(&block)
  puts block.class
  block.call
  thrice(&block)
  thrice(&block)
end

seven_times { x += 10 }
# Proc
#=> 73
```
