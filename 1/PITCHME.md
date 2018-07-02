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

#### Installing Ruby

Rule #1 - **DO NOT** use `apt` or any other built-in package manager to install
Ruby.

Use `rvm`, or `rbenv`, or just like me - <br>
`ruby-install`+`chruby`, as it is the simplest toolset of them all.

We will use the following guide: <br>
https://ryanbigg.com/2014/10/ubuntu-ruby-ruby-install-chruby-and-yo

---

#### Numbers (Integer & Float)

```ruby
1 + 2
#=> 3

1 + 2.0
#=> 3.0

1 / 2
#=> 0

1.0 / 2
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

+++

#### Extreme numbers

```ruby
100_000_000**10
#=> 100000000000000000000000000000000000000000000000000000000000000000000000000000000
```

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
@[7-15](Multi-line concatenation)
@[16-17](Initialize variable)
@[19-23](What if concatenate it with "+")
@[25-29](What if concatenate it with "<<" operator)
@[31-32](".concat" method)

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

@[1,2](Initialize variable)
@[4-7](Get single character)
@[9,10](Using regex)
@[12,13]()
@[15-18](Using ranges)

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

#### Arrays
Check this out: <br>
https://ruby-doc.org/core-2.5.1/Array.html

```ruby
Array.new
#=> []

Array.new(3)
#=> [nil, nil, nil]

Array.new(3, 'pow')
#=> ["pow", "pow", "pow"]

Array.new(4) { |i| i.to_s }
#=> ["0", "1", "2", "3"]

['pow', Array.new(2), [1, 2], 3.14]
#=> ["pow", [nil, nil], [1, 2], 3.14]

%w(monkey fish lion dog cat #{Time.now})
#=> ["monkey", "fish", "lion", "dog", "cat", "\#{Time.now}"]

%W(monkey fish lion dog cat #{Time.now})
#=> ["monkey", "fish", "lion", "dog", "cat", "2013-05-03 12:24:42 +0300"]
```

@[1,2]()
@[4,5]()
@[7,8]()
@[10,11]()
@[13,14]()
@[16-19]()

+++

#### Elements accessing

_["Ruby", "JavaScript", "Python", "Scala"]_

```ruby
languages = 'Ruby', 'JavaScript', 'Python', 'Scala'
#=> ["Ruby", "JavaScript", "Python", "Scala"]

languages[0]
#=> "Ruby"

languages.at(0)
#=> "Ruby"

languages[1]
#=> "JavaScript"

languages[4]
#=> nil

languages[2..3]
#=> ["Python", "Scala"]

languages.take(3)
#=> ["Ruby", "JavaScript", "Python"]

languages[1] = "CoffeeScript"
#=> "CoffeeScript"
languages
#=> ["Ruby", "CoffeeScript", "Python", "Scala"]
```
@[1-2]()
@[4-11]()
@[13-14]()
@[16-17]()
@[19-20]()
@[22-25]()

+++

#### Adding elements to array

_["Ruby", "JavaScript", "Python", "Scala"]_

```ruby
languages.push("Haskell")
#=> ["Ruby", "JavaScript", "Python", "Scala", "Haskell"]

languages << "Assembler"
#=> ["Ruby", "JavaScript", "Python", "Scala", "Haskell", "Assembler"]

> languages.unshift("C++")
#=> ["C++", "Ruby", "JavaScript", "Python", "Scala", "Haskell", "Assembler"]

languages.insert(3, "CoffeeScript")
#=> ["C++", "Ruby", "JavaScript", "CoffeeScript", "Python", "Scala", "Haskell", "Assembler"]

languages.insert(4, "Haml", "Sass")
#=> ["C++", "Ruby", "JavaScript", "CoffeeScript", "Haml", "Sass", "Python", "Scala", "Haskell", "Assembler"]

```
@[1-2](Push)
@[4-5](Push operator)
@[7-8](Unshift)
@[10-14](Insert)
