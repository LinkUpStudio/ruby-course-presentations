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
end
```
@[3-10]()
@[8-9](Don't forget to use communicative variable name)
@[4-7](It looks like you want to initialize default values. A good solution will look like this:)

+++

###### 10_temperature_object

```ruby
def initialize(opts = {})
  opts = defaults.merge(opts)
  @fahrenheit = opts[:f]
  @celsius = opts[:c]
end

private

def defaults
  { f: nil, c: nil }
end
```

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
  attr_reader :in_fahrenheit, :in_celsius

  def initialize(f: nil, c: nil)
    self.in_fahrenheit = f
    self.in_celsius = c
  end

  def self.from_celsius(celsius)
    new(c: celsius)
  end

  def self.from_fahrenheit(fahrenheit)
    new(f: fahrenheit)
  end

  def in_fahrenheit=(fahrenheit)
    fahrenheit ? (@in_fahrenheit = fahrenheit) : return
    @in_celsius = (fahrenheit - 32) * 5 / 9.0
  end

  def in_celsius=(celsius)
    celsius ? (@in_celsius = celsius) : return
    @in_fahrenheit = celsius * 9 / 5.0 + 32
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



