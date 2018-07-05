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
@[1-6]()
@[8-16]()
@[18-19]()
@[21-22]()

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
@[1-2]()
@[3-4]()
@[6-7]()
@[8-9]()
@[10-11]()

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
@[1-2]()
@[3-4]()
@[6-7]()
@[8-9]()
@[10-11]()

+++

#### @css[ruby-red](Low priority) Boolean operators
@css[ruby-red](Better don't play with them)

```ruby
a = nil or 99     #=> 99
a                 #=> nil

a = 99 and nil    #=> nil
a                 #=> 99
```
@[1-2]()
@[4-5]()

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
