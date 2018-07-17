# HOMEWORK 2

---

###### 10_temperature_object

Too much logic in a constructor is bad

```ruby
def initialize(hash)
  @farenheit = hash[:f]
  @celsium = hash[:c]
  @farenheit ||= (@celsium.to_f * 9 / 5) + 32 unless @celsium.nil?
  @celsium = (@farenheit - 32) * 5 / 9 unless @farenheit.nil?
end
```

+++

###### 10_temperature_object

```ruby
class Temperature
  attr_accessor :f, :c
  def initialize(opts = nil)
    opts = {
        f:  nil,
        c:  nil
    }.merge(opts || {})
    @f = opts[:f]
    @c = opts[:c]
  end

  ...

  def initialize(opts = {})
    @f = opts[:f]
    @c = opts[:c]
  end
end
```
@[8-9](Don't forget to use communicative variable name)
@[4-7](Hashes have a default value that is returned when accessing keys that do not exist in the hash. If no default is set nil is used.)
@[14-17](We need just set a default value for attributes of a constructor)

+++

###### 10_temperature_object

```ruby
def initialize(opts = {})
  @fahrenheit = opts[:f]
  @celsius = opts[:c]
end

def in_fahrenheit
  @fahrenheit ||= ((@celsius * 1.8) + 32)
end

def in_celsius
  @celsius ||= ((@fahrenheit.to_f - 32) / 1.8).round
end
```
@[6-12](Also, here we have a bug)

+++

###### 10_temperature_object

GOOD

```ruby
class Temperature
  def initialize(options)
    @in_fahrenheit = options[:f] || options[:c] * (9.0 / 5) + 32
  end

  attr_reader :in_fahrenheit

  def self.from_celsius(degrees_celsius)
    new(c: degrees_celsius)
  end

  def self.from_fahrenheit(degrees_fahrenheit)
    new(f: degrees_fahrenheit)
  end

  def in_celsius
    @in_celsius ||= (@degrees_fahrenheit - 32) * (5.0 / 9)
  end
end

class Celsius < Temperature
  def initialize(celsius)
    super(c: celsius)
  end
end

class Fahrenheit < Temperature
  def initialize(fahrenheit)
    super(f: fahrenheit)
  end
end

```

---

###### 14_array_extensions

`#sum`

not good

```ruby
def sum
  self.length == 0 ? 0 : self.inject(:&+)
end
```
@[2-2](use `inject(0, :+)` instead of `reduce(0, &:+)` it's a little bit faster))

+++

###### 14_array_extensions

`#sum`

good

```ruby
def sum
  self.inject(0, :+)
end
```

+++

###### 14_array_extensions

`#reduce`

use `reduce(0, :+)` instead of `reduce(0, &:+)` it's a little bit faster

+++

###### 14_array_extensions

`#sum`

The best solution is to use ready solutions

`https://goo.gl/jixQ9u`

+++

###### 14_array_extensions

not good

```ruby
def square
  self.length == 0 ? self : self.map{ |num| num**2 }
end
def square!
  self.length == 0 ? self : self.map!{ |num| num**2 }
end
```

+++

###### 14_array_extensions

`#square`, `#square!`

GOOD

```ruby
def square
  self.map { |arg| arg**2 }
end

def square!
  self.map! { |arg| arg**2 }
end
```

+++

###### 14_array_extensions

`#square`, `#square!`

COOL

```ruby
class Array
  def square
    map(&square_proc)
  end

  def square!
    map!(&square_proc)
  end

  private

  def square_proc
    proc { |value| value**2 }
  end
end

```

---