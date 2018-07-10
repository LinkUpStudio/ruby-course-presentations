# Introduction to @css[ruby-red no-text-transform](Ruby)
### @css[no-text-transform](pt. 3)

---

#### Method visibility

All methods are `public` by **default**.

You can also use `protected` and `private`.

+++

Protected

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def together_price(other)
    discount_price + other.discount_price
  end

  protected

  def discount_price
    (price * 0.9).round(2)
  end
end

book = Product.new('Book', 8)
book.discount_price                 # => NoMethodError

magazine = Product.new('Magazine', 5)
book.together_price(magazine)       # => 11.7
```
@[4-7]()
@[9-11]()
@[13-17]()
@[20-21]()
@[23-24]()

+++

Private

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def full_cost
    price + tax
  end

  private

  def tax
    @tax ||= (price * 0.2).round(2)
  end
end

book = Product.new('Book', 8)
book.tax                            # => NoMethodError
book.full_cost                      # => 9.6
```
@[9-11]()
@[13-17]()
@[20-22]()

+++

`private` is too private sometimes

```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  def non_working_full_cost
    price + self.tax
  end

  private

  def tax
    @tax ||= (price * 0.2).round(2)
  end
end

book = Product.new('Book', 8)
book.non_working_full_cost          # => NoMethodError
```
@[9-11]()
@[20-21]()


+++

but `private` is OK for **setters**

```ruby
class Product
  attr_reader :name, :price, :discount

  def initialize(name, price)
    @name = name
    @price = price
    self.discount = 0
  end

  def full_cost
    price + tax - discount
  end

  def make_discounted!
    self.discount = price / 3.0
    self
  end

  private

  def discount=(value)
    @discount = value.round(2)
  end

  def tax
    @tax ||= (price * 0.2).round(2)
  end
end

book = Product.new('Book', 8)
book.full_cost                    # => 9.6
book.make_discounted!.full_cost   # => 6.93
book.full_cost                    # => 6.93
```
@[2]()
@[4-8]()
@[10-12]()
@[14-17]()
@[19-23]()
@[30-33]()
