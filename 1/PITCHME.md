# Introduction to Ruby

---

### @fa[diamond ruby-red] @css[ruby-red](Ruby is...)
A dynamic, open source programming language with a focus on simplicity and
productivity. It has an elegant syntax that is natural to read and easy to
write.

+++

### @fa[diamond ruby-red] @css[ruby-red](Ruby это...)
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

Use `rvm`, or `rbenv`, or just like me - `ruby-install`+`chruby`, as it is the simplest toolset of them all.

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

'#{6 * 6}'
#=> "\#{6 * 6}"

"#{6 * 6}"
#=> "36"

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

'This string is so long, it does not fit nicely into your code, ' \
'so you can use this "multiline" concatenation technique, and deserve ' \
'a cookie for being nice.'
#=> "This string is so long, it does not fit nicely into your code, so you can use this \"multiline\" concatenation technique, and deserve a cookie for being nice."

str = 'Luke'
#=> "Luke"

str << ', I am your father'
#=> "Luke, I am your father"

str.concat(', LOL')
#=> "Luke, I am your father, LOL"
```

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
```
