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
@[4-7](Hash has by default values nil.)
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
@[1-5](But in our case, this will be enough)
@[6-12](also, here we have a bug)

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

GOOD

```ruby
def square
  self.map { |arg| arg * arg }
end

def square!
  self.map! { |arg| arg*arg }
end
```

+++


```ruby

COOL

class Array
  SQUARE_PROC = proc { |value| value**2 }

  def square
    map(&SQUARE)
  end

  def square!
    map!(&SQUARE)
  end

  private

  def square_proc
    proc { |value| value**2 }
  end
end

```