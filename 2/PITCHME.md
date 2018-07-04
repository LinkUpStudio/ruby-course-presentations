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

### Case, when

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

### Case-when with parameter

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
