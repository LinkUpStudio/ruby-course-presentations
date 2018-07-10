# HOMEWORK

---

#### 00_hello



---

#### 01_temperature



---

#### 02_calculator

##### #add, #subtract

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

#### 02_calculator

##### #sum

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

#### 02_calculator

##### #power

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

#### 02_calculator

##### #multiply

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

#### 02_calculator

##### #factorial best solutions

```ruby
def factorial(number)
  (1..number).reduce(1, :*)
end

def factorial(numb)
  numb.downto(1).reduce(:*)
end
```

---

#### 03_simon_says

#### #echo, #shout

```ruby
def echo(string)
  string
end

def shout(string)
  string.upcase
end
```

+++

#### 03_simon_says

#### #repeat

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

#### 03_simon_says

#### #echo, #shout

```ruby
def echo(string)
  string
end

def shout(string)
  string.upcase
end
```

+++


---


#### 04_pig_latin


---

#### 05_silly_blocks



---

#### 06_performance_monitor



---

#### 07_hello_friend



---

#### 08_book_titles


---

#### 09_timer

```ruby

```

@[1-2]()
@[4-11]()

