# Introduction to @css[ruby-red no-text-transform](Ruby)

---

### @fa[diamond ruby-red] @css[ruby-red no-text-transform](Ruby is...)
A dynamic, open source programming language with a focus on simplicity and
productivity. It has an elegant syntax that is natural to read and easy to
write.

+++

### @fa[diamond ruby-red] @css[ruby-red no-text-transform](Ruby это...)
Динамический язык программирования с открытым исходным кодом с упором на
простоту и продуктивность. Он обладает элегантным синтаксисом, который приятно
читать и легко писать.

---

#### ... and everything is an object

```ruby
'I am calm'.upcase
#=> "I AM CALM"

3.next
#=> 4

true.class
#=> TrueClass
nil.class
#=> NilClass

[1, 2, 3].reverse
#=> [3, 2, 1]
```

---

#### Join our Slack!

https://goo.gl/oL9Vy8

"oh, big L, nine, big V, why, eight"

---

#### Installing Ruby

Rule #1 - **DO NOT** use `apt` or any other built-in package manager to install
Ruby.

Use `rvm`, or `rbenv`, or just like me - <br>
`ruby-install`+`chruby`, as it is the simplest toolset of them all.

We will use the following guide: <br>
https://ryanbigg.com/2014/10/ubuntu-ruby-ruby-install-chruby-and-you

---

#### Numbers (Integer & Float)

```ruby
1 + 2
#=> 3

1 + 2.0
#=> 3.0

1.0 / 2
#=> 0.5

1 / 2
#=> 0

one = 1
#=> 1
two = 2
#=> 2
one / two.to_f
#=> 0.5

10.0 % 3
#=> 1.0

3**2
#=> 9

Math.sqrt(9)
#=> 3.0

1_000_000_000
#=> 1000000000
```
@[1-5]()
@[7-11](Division gotcha!)
@[13-18](What if we got 2 integer variables, but need correct division)
@[20-21](Remaining from division)
@[23-24](Power operation)
@[26-27](Squire root)
@[29-30](Readable syntax for big numbers)

+++

#### Extreme numbers

```ruby
100_000_000**10
#=> 100000000000000000000000000000000000000000000000000000000000000000000000000000000
```

+++

#### Float is NOT to count money!

```ruby
0.1 + 0.5
#=> 0.6
0.1 + 0.2
#=> 0.30000000000000004
```

Count money in **cents** instead.

---

#### Strings

```ruby
String.new
#=> ""

''
#=> ""

'Na ' * 5
#=> "Na Na Na Na Na "

"#{6 * 6}"
#=> "36"

'#{6 * 6}'
#=> "\#{6 * 6}"

%q(<p class='quote'>"What did you say?"<p>)
#=> "<p class='quote'>\"What did you say?\"<p>"

%q(#{6 * 6})
#=> "\#{6 * 6}"

%Q(#{6 * 6})
#=> "36"
```
@[1-5](Creation of blank String)
@[7,8](Strings can be multiplied)
@[10-14](Difference between single and double quotes)
@[16-23](Alternative "percent" syntax)

+++

#### String concatenation

```ruby
'Con' 'cat' 'ena' 'te'
#=> "Concatenate"

'Con' + 'cat' + 'ena' + 'te'
#=> "Concatenate"

'This string is so long, it does not fit nicely into your code, so you can use this "multiline" concatenation technique, and deserve a cookie for being nice.'
#=> "This string is so long, it does not fit nicely into your code, so you can use this \"multiline\" concatenation technique, and deserve a cookie for being nice."

'This string is so long, it does not fit nicely ' \
'into your code, so you can use this "multiline" ' \
'concatenation technique, and deserve ' \
'a cookie for being nice.'
#=> "This string is so long, it does not fit nicely into your code, so you can use this \"multiline\" concatenation technique, and deserve a cookie for being nice."

str = 'Luke'
#=> "Luke"

str + ', I am your father'
#=> "Luke, I am your father"

str
#=> "Luke"

str << ', I am your father'
#=> "Luke, I am your father"

str
#=> "Luke, I am your father"

str.concat(', LOL')
#=> "Luke, I am your father, LOL"
```

@[1-5](Simplest ways of concatenation)
@[7-15]("Multi-line" concatenation)
@[16-17](Variable initialization)
@[19-23](Non object changing concatenation)
@[25-29](Object changing concatenation)
@[31-32](".concat" method is same as "<<" operator)

+++

#### String accessing

```ruby
str = 'Luke, I am your father'
#=> "Luke, I am your father"

str[0]
#=> "L"
str[-1]
#=> "r"

str[/\w+r/]
#=> "your"

str[11, 4]
#=> "your"

str[0..4]
#=> "Luke,"
str[0...4]
#=> "Luke"
```

@[1,2](Variable initialization)
@[4-7](Getting a single character)
@[9,10](Getting sub-string by regex)
@[12,13](Getting sub-string by start position and number of characters)
@[15-18](Getting sub-string using range)

+++

#### String has a lot of useful methods
Check this out: <br>
https://ruby-doc.org/core-2.5.1/String.html <br>
(Or just google *"ruby string"* **or any other class**)

```ruby
'mesSed UP CASE'.capitalize
#=> "Messed up case"

'Luke, I am your father'.index('am')
#=> 8

'Luke, I am your father'.split
#=> ["Luke,", "I", "am", "your", "father"]

'Luke, I am your father'.split(', ')
#=> ["Luke", "I am your father"]
```

@[1,2](Capitalization)
@[4,5](Find index)
@[7-11](Split by)

---

#### Ranges
https://ruby-doc.org/core-2.5.1/Range.html

```ruby
(-1..-5).to_a      #=> []
(-5..-1).to_a      #=> [-5, -4, -3, -2, -1]
('a'..'e').to_a    #=> ["a", "b", "c", "d", "e"]
('a'...'e').to_a   #=> ["a", "b", "c", "d"]
```
@[1](Wrong range, works in ASCending order only)
@[2,3](Inclusive range)
@[4](Exclusive range)

---

#### Symbols
https://ruby-doc.org/core-2.5.1/Symbol.html

```ruby
:fast_and_furious
#=> :fast_and_furious
:'fast_and_furious'
#=> :fast_and_furious

:*
#=> :*

:'Frodo Baggins'
#=> :"Frodo Baggins"

:'-#{6 * 6}-'
#=> :"-\#{6 * 6}-"
:"-#{6 * 6}-"
#=> :"-36-"
```
@[1-4](Normal Symbol literals)
@[6-7](Some single characters may also be used, like + - * / % ^ & |)
@[9-10](Any other strings may also be used)
@[12-15](And of course, interpolation)

+++

#### Symbols are always same (like numbers)

```ruby
:Frodo.object_id
#=> 1283748
:Frodo.object_id
#=> 1283748
1.object_id
#=> 3
1.object_id
#=> 3

'Frodo'.object_id
#=> 47384436502040
'Frodo'.object_id
#=> 47384436496380

'Frodo'.to_sym
#=> :Frodo
'Frodo'.to_sym.object_id
#=> 1283748
```
@[1-8](:Frodo is always the same :Frodo, just like 1)
@[10-13](While 'Frodo' each time is DIFFERENT object in memory)
@[15-18](Any String can be converted to Symbol, and still same :Frodo)

---

#### Arrays
https://ruby-doc.org/core-2.5.1/Array.html

```ruby
Array.new
#=> []

[]
#=> []

Array.new(3)
#=> [nil, nil, nil]

Array.new(3, 'pow')
#=> ["pow", "pow", "pow"]

Array.new(4) { |i| i.to_s }
#=> ["0", "1", "2", "3"]

['pow', Array.new(2), [1, 2], 3.14]
#=> ["pow", [nil, nil], [1, 2], 3.14]
```
@[1-5](Blank Array creation)
@[7,8](Creation of Array with initial size)
@[10,11](Creation of Array with initial size and default object)
@[13,14](Using a block for Array initialization)
@[16,17](Array can contain anything, it shouldn't be objects of same class)

+++

#### Percent syntax for arrays

```ruby
%w[word Dart\ Vader #{6 * 6}]
#=> ["word", "Dart Vader", "\#{6", "*", "6}"]

%W[word Dart\ Vader #{6 * 6}]
#=> ["word", "Dart Vader", "36"]

%i[word Dart\ Vader #{6 * 6}]
#=> [:word, :"Dart Vader", :"\#{6", :*, :"6}"]

%I[word Dart\ Vader #{6 * 6}]
#=> [:word, :"Dart Vader", :"36"]
```
@[1-5](Array of strings)
@[7-11](Array of symbols)

+++

#### Elements accessing

_["Ruby", "JS", "Python", "Scala"]_

```ruby
languages = 'Ruby', 'JS', 'Python', 'Scala'
#=> ["Ruby", "JS", "Python", "Scala"]

languages[0]
#=> "Ruby"

languages.at(0)
#=> "Ruby"

languages[4]
#=> nil
languages[-100]
#=> nil

languages[2..3]
#=> ["Python", "Scala"]

languages.take(3)
#=> ["Ruby", "JS", "Python"]

languages[1] = 'CoffeeScript'
#=> "CoffeeScript"
languages
#=> ["Ruby", "CoffeeScript", "Python", "Scala"]
```
@[1-2](Initializing Array with no square brackets!)
@[4-8](Accessing elements by index)
@[10-13](No "index-out-of-range" errors!)
@[15-16](Ranges again)
@[18-19](Taking first few elements)
@[21-24](Replacing Array elements)

+++

#### A bit about "no brackets" stuff

```ruby
first, second = 'I am first!', 2, 3
#=> ["I am first!", 2, 3]
first
#=> "I am first!"
second
#=> 2

first, second = ['I am first!', 2, 3]
#=> ["I am first!", 2, 3]
first
#=> "I am first!"
second
#=> 2

first, second = second, first
#=> [2, "I am first!"]
first
#=> 2
second
#=> "I am first!"

first, second, *others, last = [1, 2, 3, 4, 5, 6]
#=> [1, 2, 3, 4, 5, 6]
first
#=> 1
second
#=> 2
others
#=> [3, 4, 5]
last
#=> 6
```
@[1-6](Multiple variables assignment)
@[8-13](Same, but with brackets)
@[15-20](Variables swapping without third variable!)
@[22-31](Hello, Splat operator! Slicing arrays use-case.)

+++

#### Adding elements to array

_["Ruby", "JS", "Python", "Scala"]_

```ruby
languages.push('Haskell')
#=> ["Ruby", "JS", "Python", "Scala", "Haskell"]

languages << 'C#'
#=> ["Ruby", "JS", "Python", "Scala", "Haskell", "C#"]

languages.unshift('C++')
#=> ["C++", "Ruby", "JS", "Python", "Scala", "Haskell", "C#"]

languages.insert(3, 'Elm')
#=> ["C++", "Ruby", "JS", "Elm", "Python", "Scala", "Haskell", "C#"]

languages.insert(4, 'Haml', 'Sass')
#=> ["C++", "Ruby", "JS", "Elm", "Haml", "Sass", "Python", "Scala", "Haskell", "C#"]
```
@[1-2](Push, e.g. add to end)
@[4-5](Append operator, same as push)
@[7-8](Unshift - add to beginning)
@[10-14](Insert)

+++

#### Removing elements from array

```ruby
languages = ['C++', 'Ruby', 'JS', 'Elm', 'Haml', 'Sass', 'Python', 'Scala']
#=> ["C++", "Ruby", "JS", "Eml", "Haml", "Sass", "Python", "Scala"]

languages.pop
#=> "Scala"
languages
#=> ["C++", "Ruby", "JS", "Elm", "Haml", "Sass", "Python"]

languages.shift
#=> "C++"
languages
#=> ["Ruby", "JS", "Elm", "Haml", "Sass", "Python"]

languages.delete_at(2)
#=> "Elm"
languages
#=> ["Ruby", "JS", "Haml", "Sass", "Python"]

languages.delete('Sass')
#=> "Sass"
languages
#=> ["Ruby", "JS", "Haml", "Python"]

languages = 'Ruby', 'JS', nil, 0, 'Python', nil
#=> ["Ruby", "JS", nil, 0, "Python", nil]
languages.compact
#=> ["Ruby", "JS", 0, "Python"]

languages = 'Ruby', 'JS', 'Elm', 'Python', 'Elm'
#=> ["Ruby", "JS", "Elm", "Python", "Elm"]
languages.uniq
#=> ["Ruby", "JS", "Elm", "Python"]
```
@[1-2]()
@[4-7](Pop, e.g. remove last)
@[9-12](Shift - remove first)
@[14-17](Delete by index)
@[19-22](Delete particular object)
@[24-27](Compact - clear from nils)
@[29-32](Uniq - cleat from duplicates)

+++

#### Obtaining information about array

```ruby
languages = 'Ruby', 'JavaScript', 'Scala', 'Python', 'Scala'
#=> ["Ruby", "JavaScript", "Scala", "Python", "Scala"]

languages.length
#=> 5
languages.count
#=> 5
languages.size
#=> 5

languages.empty?
#=> false

languages.include?('Ruby')
#=> true

languages.include?('PHP')
#=> false
```
@[1-2]()
@[4-9](Number of elements)
@[11-12](Is empty?)
@[14-18](Includes particular object?)

+++

#### Arrays concatenation 

```ruby
days1 = ['Mon', 'Tue', 'Wed']
#=> ["Mon", "Tue", "Wed"]
days2 = ['Thu', 'Fri', 'Sat', 'Sun']
#=> ["Thu", "Fri", "Sat", "Sun"]

days1 + days2
#=> ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
days1
#=> ["Mon", "Tue", "Wed"]

days1.concat(days2)
#=> ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
days1
#=> ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]

days1 = ['Mon', 'Tue', 'Wed']
#=> ["Mon", "Tue", "Wed"]
days1 << 'Thu' << 'Fri' << 'Sat' << 'Sun'
#=> ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
```
@[1-4]()
@[6-9](Non object changing concatenation)
@[11-14](Object changing concatenation)
@[16-19](Multiple append operators, also changing)

+++

#### Operations with arrays

```ruby
some_countries = %w[Norway Canada Ukraine Japan]
#=> ["Norway", "Canada", "Ukraine", "Japan"]
some_europe_countries = %w[Norway Spain Ukraine]
#=> ["Norway", "Spain", "Ukraine"]

some_countries | some_europe_countries
#=> ["Norway", "Canada", "Ukraine", "Japan", "Spain"]

some_countries & some_europe_countries
#=> ["Norway", "Ukraine"]

some_countries - some_europe_countries
#=> ["Canada", "Japan"]

some_europe_countries - some_countries
#=> ["Spain"]

some_europe_countries + ['Italy']
#=> ["Norway", "Spain", "Ukraine", "Italy"]

some_europe_countries * 2
#=> ["Norway", "Spain", "Ukraine", "Norway", "Spain", "Ukraine"]

some_europe_countries * '/'
#=> "Norway/Spain/Ukraine"

some_europe_countries.join('/')
#=> "Norway/Spain/Ukraine"
```
@[1-4]()
@[6-7](Union)
@[9-10](Intersection)
@[12-16](Difference, not commutative!)
@[18-19](Addition)
@[21-22](Multiplication)
@[24-28](Joining into single string by given "joining" string)

+++

#### Iterators for arrays

```ruby
a = %w[a b c]
#=> ["a", "b", "c"]

a.each { |x| print x, ' -- ' }
# a -- b -- c -- => ["a", "b", "c"]

a.each_index { |x| print x, ' -- ' }
# 0 -- 1 -- 2 -- => ["a", "b", "c"]

a.each_with_index { |item, index| puts "#{index}: #{item}" }
# 0: a
# 1: b
# 2: c
#=> ["a", "b", "c"]

a.map { |x| x.upcase }
#=> ["A", "B", "C"]
a
#=> ["a", "b", "c"]
```
@[1-2]()
@[4-5](Each - applies given block to each element, returns initial array)
@[7-8](Each index)
@[10-14](Each with index)
@[16-19](Map - same as each, but returns array of results)

---

#### Hash
Check this out: <br>
http://ruby-doc.org/core-2.5.1/Hash.html

```ruby
{:font_size => 10, :font_family => "Arial"}
#=> {:font_size=>10, :font_family=>"Arial"}

{font_size: 10, font_family: 'Arial'}
#=> {:font_size=>10, :font_family=>"Arial"}

Hash.new
#=> {}

h = Hash.new('Default value')
#=> {}
h['key']
#=> "Default value"

h = Hash.new { |hash, key| hash[key] = "Default value: #{key}" }
#=> {}
h['key']
#=> "Default value: key"

h = Hash.new
h.default = 'Default value'
h['key']
#=> "Default value"
```
@[1-2](Hash creation)
@[4-5](Hash creation)
@[7-8](Hash creation)
@[10-13](Default value)
@[15-18](Default value)
@[20-23](Default value)

+++

#### Hash elements deleting

```ruby
h = {'a' => 100, 'b' => 200}
#=> {"a"=>100, "b"=>200}
h.delete('a')
#=> 100
h.delete('z')
#=> nil

h = {'a' => 100, 'b' => 200, 'c' => 300}
#=> {"a"=>100, "b"=>200, "c"=>300}
h.delete_if { |key, value| value > 100 }  
#=> {"a"=>100}

h = {'a' => 100, 'b' => 200, 'c' => 300}
#=> {"a"=>100, "b"=>200, "c"=>300}
h.keep_if {|key, value| value > 100 }
#=> {"b"=>200, "c"=>300}

h = {1 => 'a', 2 => 'b', 3 => 'c'}
#=> {1=>"a", 2=>"b", 3=>"c"}
h.shift
#=> [1, "a"]
h
#=> {2=>"b", 3=>"c"}
```
@[1-6](Delete)
@[8-11](Delete if)
@[13-16](Keep if)
@[18-23](Shift)

+++

#### Iterators for hash

```ruby
h = {'a' => 100, 'b' => 200}
#=> {"a"=>100, "b"=>200}

h.each { |key, value| puts "#{key} is #{value}" }
# a is 100
# b is 200
# => {"a"=>100, "b"=>200}

h.each_key { |key| puts key }
# a
# b
#=> {"a"=>100, "b"=>200}

h.each_value { |value| puts value }
# 100
# 200
# => {"a"=>100, "b"=>200}
```
@[1-2]()
@[4-7](Each)
@[9-12](Each key)
@[14-17](Each value)

+++

#### Hash
_{"a" => 100, "b" => 200, "c" => 300}_

```ruby
h = {'a' => 100, 'b' => 200, 'c' => 300}
#=> {"a"=>100, "b"=>200, "c"=>300}
 
h.key?('a')
#=> true

h.key?('z')
#=> false

h.value?(100)
#=> true
 
h.value?(999)
#=> false

h.keys
#=> ["a", "b", "c"]

h.values
#=> [100, 200, 300]

h.values_at('a', 'c')
#=> [100, 300]

h.select { |key, value| value > 100 }
#=> {"b" => 200, "c" => 300}

h.length
#=> 3

h.delete('a')
#=> 100

h.length
#=> 2

h1 = {'a' => 100, 'b' => 200}
#=> {"a"=>100, "b"=>200}

h2 = {'b' => 254, 'c' => 300}
#=> {"b"=>254, "c"=>300}

h1.merge(h2)
#=> {"a"=>100, "b"=>254, "c"=>300}
```
@[1-2]()
@[4-8](Returns true if the given key is present in hash)
@[10-14](Returns true if the given value is present for some key in hash)
@[16-17](Returns a new array populated with the keys from hash)
@[19-20](Returns a new array populated with the values from hash)
@[22-23](Return an array containing the values associated with the given keys)
@[25-26](Returns a new hash consisting of entries for which the block returns true)
@[28-35](Returns the number of key-value pairs in the hash)
@[37-44](Returns a new hash containing the contents of other_hash and the contents of hash)

---

#### Time

http://ruby-doc.org/core-2.5.1/Time.html

```ruby
t = Time.new
#=> 2018-06-22 15:52:16 +0300

t.year
#=> 2018
 
t.month
#=> 6
 
t.day
#=> 22
 
t.wday
#=> 5
 
t.yday
#=> 173
 
t.hour
#=> 15
 
t.min
#=> 52
 
t.sec
#=> 16
 
t.zone
#=> "EEST"

t.strftime('%Y-%m-%d %H:%M:%S')
#=> "2018-06-22 15:52:16"

t.sunday?
#=> false
```
@[1-2]()
@[4-11]()
@[13-17]()
@[19-26]()
@[28-29](Returns the name of the time zone used for time)
@[31-32](Formats time according to the directives in the given format string)
@[34-35](Returns true if time represents Sunday)

---

#### To be continued...
