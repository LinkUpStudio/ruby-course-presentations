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
    (@price * 0.9).round(2)
  end
end

book = Product.new('Book', 8)
book.discount_price                 # => NoMethodError

magazine = Product.new('Magazine', 5)
book.together_price(magazine)       # => 12.59
```
@[1-18]()
@[9-17]()
@[20-21]()
@[23-24]()
