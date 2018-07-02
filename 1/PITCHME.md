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
https://ryanbigg.com/2014/10/ubuntu-ruby-ruby-install-chruby-and-you

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

#### Arrays
Check this out: <br>
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

languages[1]
#=> "JS"

languages[4]
#=> nil

languages[2..3]
#=> ["Python", "Scala"]

languages.take(3)
#=> ["Ruby", "JS", "Python"]

languages[1] = 'CoffeeScript'
#=> "CoffeeScript"
languages
#=> ["Ruby", "CoffeeScript", "Python", "Scala"]

languages[100]
#=> nil
languages[-100]
#=> nil
```
@[1-2]()
@[4-11]()
@[13-14]()
@[16-17]()
@[19-20]()
@[22-25]()
@[27-30](No "index-out-of-range" errors!)

+++

#### Adding elements to array

_["Ruby", "JS", "Python", "Scala"]_

```ruby
languages.push('Haskell')
#=> ["Ruby", "JS", "Python", "Scala", "Haskell"]

languages << 'C#'
#=> ["Ruby", "JS", "Python", "Scala", "Haskell", "C#"]

> languages.unshift('C++')
#=> ["C++", "Ruby", "JS", "Python", "Scala", "Haskell", "C#"]

languages.insert(3, 'Elm')
#=> ["C++", "Ruby", "JS", "Elm", "Python", "Scala", "Haskell", "C#"]

languages.insert(4, 'Haml', 'Sass')
#=> ["C++", "Ruby", "JS", "Elm", "Haml", "Sass", "Python", "Scala", "Haskell", "C#"]
```
@[1-2](Push)
@[4-5](Push operator)
@[7-8](Unshift)
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
@[1-2](Pop)
@[4-7](Pop)
@[9-12](Shift)
@[14-17](Delete at)
@[19-22](Delete)
@[24-27](Compact)
@[29-32](Uniq)
