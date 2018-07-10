# HOMEWORK 1

---

###### 00_hello

BAD :)

```ruby
def hello
  'Hello!'
end

def greet(who)
      "Hello, #{who}!"
end
```

+++

###### 00_hello

GOOD

```ruby
def hello
  'Hello!'
end

def greet(name)
  "Hello, #{name}!"
end
```

---

###### 01_temperature

##### Solution 1 (BAD)

```ruby
def ftoc(t)
  t=((t-32.0)*5.0)/9.0

end
def ctof(t)
  t=t* 9.0/5.0 + 32

end
```

+++

###### 01_temperature

##### Solution 2 
some of ".0" are not necessary

```ruby
def ftoc(temperature)
  (temperature - 32.0) / 9.0 * 5.0
end

def ctof(temperature)
  temperature * 9.0 / 5.0 + 32.0
end
```

+++

###### 01_temperature

##### Solution 3 (Monkey Patching)
https://www.culttt.com/2015/06/17/what-is-monkey-patching-in-ruby/

```ruby
class Float
  def to_big_i
    (self + 0.5).to_i
  end
end

def ftoc(temperature)
  temperature = (temperature - 32) / (9.0 / 5.0)
  temperature.to_big_i
end

def ctof(temperature)
  temperature = temperature * (9.0 / 5) + 32
end
```

+++

###### 01_temperature

##### Solution 4

```ruby
def ftoc (fahrenheit)
  (fahrenheit - 32) * 5 / 9.0
end

def ctof (celsius)
  celsius * 9 / 5.0 + 32
end
```

---

###### 02_calculator

`#add`, `#subtract`

Well done! Just don't forget about spacing.

Following example as a GOOD one:

```ruby
def add(first_num, second_num)
  first_num + second_num
end

def subtract(first_num, second_num)
  first_num - second_num
end
```

+++

###### 02_calculator

`#sum`

```ruby
def sum(array)
  array.sum
end

def sum(array)
  if array.length == 0
    0
  else
    array.inject(:+)
  end
end

def sum (args)
  return 0 if args.empty?
  args.reduce :+
end

def sum(arr)
  arr.inject(0) { |sum, x| sum + x }
end
```
@[1-3]()
@[5-11](In such cases please use #empty?)
@[13-16](Read in style guide about using parentheses https://github.com/rubocop-hq/ruby-style-guide#method-invocation-parens)
@[18-20](Best solution)

+++

###### 02_calculator

`#power`

```ruby
def power(powed, power)
  return powed**power
end

def power(num_1, num_2)
  num_1**num_2
end
```
@[1-3](Something is redundant...)
@[5-7](Good)

+++

###### 02_calculator

`#multiply`

##### multiply(1, 2, 3)

```ruby
def multiply(*arr)
  a = 1
  arr.each { |x| a = x * a }
  return a
end

def multiply(num_1=1, num_2=1)
  if num_1.is_a?(Integer)
    num_1*num_2
  else
    num_1.inject(:*)
  end
end

def multiply(first_num, second_num, *array)
  first_num * second_num * array.reduce(1, :*)
end

def multiply(*args)
  args.reduce(:*)
end
```
@[1-17]()
@[19-21](Best solution)

+++

###### 02_calculator

`#factorial` best solutions

```ruby
def factorial(number)
  (1..number).reduce(1, :*)
end

def factorial(numb)
  numb.downto(1).reduce(:*)
end
```

---

###### 03_simon_says

`#echo`, `#shout`

```ruby
def echo(string)
  string
end

def shout(string)
  string.upcase
end
```

+++

###### 03_simon_says

`#repeat`

```ruby
def repeat(string, mul = 2)
  a = 0
  b = ''
  while a < mul
    a += 1
    b.concat(string, ' ')
  end
  b = b.rstrip
  b
end

def repeat(word, repeat = 2)
  ((word + ' ') * (repeat.to_i - 1)) + word
end

def repeat(something, r=2)
  ([something] * r).join ' '
end

def repeat(words, count_of_repeat=2)
  ((words + ' ') * count_of_repeat).chop
end

def repeat(string, repeat_count = 2)
  ((string + ' ') * repeat_count).strip
end
```
@[1-10](In this case #times better then while)
@[12-14](Hard to read)
@[16-18](Good, but where are spaces and parentheses?)
@[20-26](Good solutions. In this case better to use #strip method)

+++

###### 03_simon_says

`#start_of_word`

```ruby
def start_of_word(word, letter)
  word[0, letter]
end

def start_of_word(word, number_of_characters)
  word[0...number_of_characters]
end
```

+++

###### 03_simon_says

`#first_word` with split

```ruby
def first_word(string)
  a = string.split
  a[0]
end

def first_word(something)
  something.split(" ")[0]
end
```
@[1-4](We can do it without variable a)
@[6-8](Good, but in this case better to use ' ' instead " ")

+++

###### 03_simon_says

`#first_word` with regex

```ruby
  string[/\b\w+\b/]
  
  string.match(/(\w+)/)[0]
  
  string.gsub(/ .*/, '')
```

+++

###### 03_simon_says

`#titleize`

##### Solution 1

```ruby
def titleize(string)
  no_capital = %w[and over the]
  arr = string.split(/ | \_| \-/)
  p arr
  arr = arr.map.with_index do |x, i|
    if no_capital.include?(x) && i != 0
      x
    else
      x.capitalize
    end
  end.join(' ')
end
```
@[4](Code should be cleaned from puts, print, p, etc.)
@[5](Does "arr=" make sense? Because it's not used after that)
@[11](It's bad to write like this. )

+++

###### 03_simon_says

`#titleize`

##### Solution 2

```ruby
def titleize(word)
  words = word.split(' ')
  action_word = words[0].capitalize  
  words[1..-1].each do |word|
    if word.downcase == 'and' || word.downcase == 'over' || word.downcase == 'the'
      action_word += ' ' + word
    else
      action_word += ' ' + word.capitalize
    end
  end
  action_word
end
```
@[5](Better to user array + #include? in such cases)
@[3-10](I think better to use #map for this)

+++

###### 03_simon_says

`#titleize`

##### Solution 3

```ruby
def titleize(sentence)
  exceptions = %w{over the and}
  titleized_sentence =  sentence.gsub(/(\b\w+\b)/) { |match| exceptions.include?(match) ? match : match.capitalize }
  titleized_sentence[0] = titleized_sentence[0].upcase
  titleized_sentence
end
```
@[3](Lines shouldn't be so loooong)
@[4](Or we can write "titleized_sentence[0].upcase!")

+++

###### 03_simon_says

`#titleize`

##### Solution 4

```ruby
def titleize(words)
  words.capitalize!
  result = ''
  array = words.split(/\s/)
  array.map {|word|
    if word.length > 4 || word == 'jaws' || word == 'kwai'
      result += word.capitalize + ' '
    else result += word + ' ' end
  }
  result.chop
end
```
@[5](Should be do..end instead {} here)
@[3-10](#map but works like #each...)

+++

###### 03_simon_says

`#titleize`

##### Solution 5

```ruby
def titleize (string)
  string.gsub(/^\S+|\S{5,}|\S+$/){|val| val.capitalize }
end
```
---

###### 04_pig_latin

##### Solution 1 ...Awesome readability

```ruby
def translate(string)
  arr = string.split
  arr.map! do |x|
  if x =~ /\A[aeiou]/i
  x.concat('ay')
  elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){3}/ || x.start_with?('sch') || x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){1}qu/
  x[3..x.size].concat(x[0..2], 'ay')
  elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){2}/ || x.start_with?('qu')
  x[2..x.size].concat(x[0..1], 'ay')
  elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){1}/
  x[1..x.size].concat(x[0], 'ay')
  end
  end
  s_res = ''
  arr.each { |x| s_res.concat(x, ' ') }
  s_res.rstrip!
end
```

+++

###### 04_pig_latin

##### Solution 1

```ruby
def translate(string)
  arr = string.split
  arr.map! do |x|
    if x =~ /\A[aeiou]/i
      x.concat('ay')
    elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){3}/ || x.start_with?('sch') || x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){1}qu/
      x[3..x.size].concat(x[0..2], 'ay')
    elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){2}/ || x.start_with?('qu')
      x[2..x.size].concat(x[0..1], 'ay')
    elsif x =~ /\A(?i:(?![a|e|i|o|u])[a-z]){1}/
      x[1..x.size].concat(x[0], 'ay')
    end
  end
  s_res = ''
  arr.each { |x| s_res.concat(x, ' ') }
  s_res.rstrip!
end
```

###### 04_pig_latin

##### Solution 2....


```ruby

```


---

###### 05_silly_blocks

`#reverser`

##### Solution 1

```ruby
def reverser
  string =  yield
  lambda do |string|
    split_string = string.split(' ')
    reversed = []
    split_string.each do |x|
      x = x.split('')
      x.size.times { reversed << x.pop }
      reversed << ' '
    end
    reversed.join.rstrip
  end.call(string)
end

def reverser
  split_string = yield.split(' ')
  split_string.map! do |word|
    chs = word.split('')
    chs.size.times { reversed << chs.pop }
  end
  split_string.join(' ').rstrip
end
```
@[1-13]()
@[15-22]()

+++

###### 05_silly_blocks

`#reverser`

##### Other solutions

```ruby
def reverser
  array = yield.split(' ')
  result = ''
  array.each{|word| result.concat(word.reverse + ' ')}
  result.chop
end

def reverser
  result = yield.split(/\b/).map(&:reverse).join
  result
end

def reverser
  yield.split(" ").map {|something| something.reverse}.join(" ")
end

def reverser
  yield.gsub(/\b\w+\b/) { |word| word.reverse }
end
```
@[1-6]()
@[8-11](We don't need variable result)
@[13-15](https://github.com/rubocop-hq/ruby-style-guide)
@[17-19](Best solution)

+++

###### 05_silly_blocks

`#adder`

```ruby
def adder(number = 1)
  yield + 1 * number
end

def adder(value = 1)
  yield + value
end
```
@[1-3]()
@[5-7]()

+++

###### 05_silly_blocks

`#repeater`

```ruby
def repeater(mul = 1)
  i = 0
  while i < mul
    i += 1
    yield
  end
end

def repeater n=1
	n.times {yield}
end

def repeater(count=1)
  count.times{
    yield(true)
  }
end

def repeater(number_of_repetitions = 1)
  number_of_repetitions.times { yield }
end
```
@[1-7](Someone reeeeally like while)
@[9-17](Oh... )
@[19-21](Best solution)

---

###### 06_performance_monitor

WAT?

```ruby
def measure(how_many_hours = 1)
  st_time = Time.now
  how_many_hours.times do |_|
    yield
  end
  end_time = Time.now
  ended = end_time - st_time
  return ended if how_many_hours == 1
  ended / how_many_hours unless how_many_hours == 1
end
```

+++

###### 06_performance_monitor

Someone really likes `#inject` :)

```ruby
def measure (n=1, &block)
  durations = []
  n.times do
    start = Time.now
    block.call
    finish = Time.now
    durations << (finish - start)
  end
  (durations.inject(0) {|sum,num| sum+num}) / n
end
````

+++

###### 06_performance_monitor

```ruby
def measure(times = 1)
  start_time = Time.now
  times.times { yield }
  (Time.now - start_time) / times
end
```

---

###### 07_hello_friend

```ruby
class Friend
  def greeting(who=nil)
    if who
  "Hello, #{who}!"
else
  "Hello!"
  end
end
end

# Other variants of #greeting method

  def greeting(who = nil)
    who ? 'Hello, ' << who << '!' : 'Hello!'
  end
  
  def greeting(friend_name='')
    if friend_name == ''
      'Hello!'
    else
      'Hello, ' + friend_name + '!'
    end
  end
  
  def greeting(name = nil)
    if name.nil?
      "Hello!"
    else
      "Hello, #{name}!"
    end
  end
```
@[1-9](Almost the best solution)
@[13-15](Hard to read)
@[17-23](Better to use interpolation)
@[25-27]("if name.nil?" can be shorter: "if name". But it's not important )

---

###### 08_book_titles

##### Solution 1

```ruby
class Book
  attr_writer :title

  def title()
    no_capital = %w[and over the a an of in]
    arr = @title.split(/ | \_| \-/)
    p arr
    arr = arr.map.with_index do |x, i|
      if no_capital.include?(x) && i != 0
        x
      else
        x.capitalize
      end
    end.join(' ')
  end
end
```
@[4](Should be without parentheses)
@[6-14](We already analized this code on some of the previous slides)

+++

###### 08_book_titles

##### Solution 2

Oh...

```ruby
class Book
  attr_accessor :title

  def title
  titleize(@title)
  end

  def skip
  [ "a", "etc", "the", "a", "for", "and" , "to", "in"]
  end

def titleize(title)
  titlew = title.capitalize.split(" ")
  titlew.collect { |word|func(word) }.join(" ")
  end

def func(word)
if skip.include?(word)
  word
else
  word.capitalize
end
end
end
```

+++

###### 08_book_titles

##### Solution 2

Lets make it more readable

```ruby
class Book
  attr_accessor :title

  def title
    titleize(@title)
  end

  def skip
    ["a", "etc", "the", "a", "for", "and" , "to", "in"]
  end

  def titleize(title)
    titlew = title.capitalize.split(" ")
    titlew.collect { |word| func(word) }.join(" ")
  end

  def func(word)
    if skip.include?(word)
      word
    else
      word.capitalize
    end
  end
end
```
@[2-6](Better to use attr_writer in this case)
@[9](Should be '' instead of "")
@[17](Bad naming)

+++

###### 08_book_titles

##### Solution 3

```ruby
class Book
  attr_reader :title

  def title=(book_title)
    words = book_title.split(' ')
    little_words = %w[the an and a of in]
    words = [words[0].capitalize] + words[1..-1].map do |word|
      if little_words.include? word
        word
      else
        word.capitalize
      end
    end
    @title = words.join(' ')
  end
end
```
@[7](As alternative we can use words[0].capitalize! and freely write words.map)

+++

###### 08_book_titles

##### Solution 4

```ruby
class Book
  def title
    return @title
  end

  def title=(book_title)
    exceptions = %w[over the and a an in of]
    @title =  book_title.gsub(/(\b\w+\b)/) { |match| exceptions.include?(match) ? match : match.capitalize }
    @title[0] = @title[0].upcase
    @title
  end
end
```
@[3](We don't need return here and better to use "attr_reader :title" in this case)

---

###### 09_timer

##### Solution 1

```ruby
class Timer
  def initialize
    @seconds = 0.0
  end
  def seconds=(seconds)
    @seconds = seconds
  end
  def seconds
    @seconds
  end
  def time_string
    @hours = @seconds / 3600
    @minutes = (@seconds % 3600) / 60
    @seconds = (@seconds % 3600 % 60)
    "%02d:%02d:%02d" % [@hours, @minutes, @seconds]
  end
end
```
@[5-10](attr_accessor :seconds)
@[12-13](I'm not sure that minutes and hours should be instance variables, because they're not used in any other methods)
@[14]()

+++

###### 09_timer

##### Solution 2

```ruby
class Timer
  def seconds=(seconds = 0.0)
    @seconds = seconds
  end

  def seconds
    return 0.0 if @seconds.nil?
    @seconds
    end

  def time_string
    return '00:00:00' if @seconds.zero?
    t = @seconds
    Time.at(t).utc.strftime('%H:%M:%S')
  end
end
```
@[5-10]()

+++

###### 09_timer

##### Solution 3

```ruby
class Timer
  attr_accessor :seconds
  def initialize
    @seconds = 0.0
  end

  def time_string
    hours = ((@seconds / 60) / 60)
    minutes = (@seconds / 60) - (hours * 60)
    second = @seconds - (minutes * 60) - (hours * 3600)
    second = '00' if second < 1
    minutes = '00' if minutes < 1
    hours = '00' if hours < 1
    format('%02d:%02d:%02d', hours, minutes, second)
  end
end
```
@[11-13](It's not really needed)

+++

###### 09_timer

##### Solution 3

```ruby
class Timer

  def initialize(seconds = 0)
    self.seconds = seconds.to_f
  end

  def seconds
    @seconds
  end

  def seconds=(seconds)
    @seconds = seconds
  end

  def time_string
    "#{padded(@seconds / 60 / 60 % 24)}:#{padded(@seconds / 60 % 60)}:#{padded(@seconds % 60)}"
  end

  def padded(number)
    '%0.2d' % number
  end

end
```
@[7-13](attr_accessor :seconds)
