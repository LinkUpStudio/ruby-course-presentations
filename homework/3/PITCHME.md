# HOMEWORK 3

---

First of all<br>
It's good to test **constructors** too

---

Let's repeat some things

+++

Describe an account when it is first opened. It has a balance of zero.<br>
-<br>
`describe` an _account_ `|` _when it is first opened_ `it` _has a balance of zero_

+++

```ruby
describe 'something' do
  context 'in one context' do
    it 'does one thing' do
    end
  end

  context 'in another context' do
    it 'does another thing' do
    end
  end
end
```
@[1](`describe` -  to wrap a set of tests against one functionality)
@[2,7](`context` - to wrap a set of tests against one functionality under the same state)
@[3,8](`it` - what should happen with (should do) described thing)

+++

Use the Ruby documentation convention of `.` (or `::`) when referring to a class method's name 
and `#` when referring to an instance method's name

```ruby
# BAD
describe 'the authenticate method for User' do
describe 'if the user is an admin' do

# GOOD
describe '.authenticate' do
describe '#admin?' do
```

+++

Do not use **should** when describing your tests. 
Use the third person in the present tense. 
Even better start using the new expectation syntax.

```ruby
# BAD
it 'should not change timings' do
  consumption.occur_at.should == valid.occur_at
end

# GOOD
it 'does not change timings' do
  expect(consumption.occur_at).to equal(valid.occur_at)
end
```

+++

When you have to assign a variable instead of using a `before` block 
**to create an instance variable**, use `let`. Using `let` the variable lazy loads 
only when it is used the first time in the test and get cached until that specific test is finished

```ruby
# this:
let(:foo) { Foo.new }

# is very nearly equivalent to this:
def foo
  @foo ||= Foo.new
end
```

+++

Depending on your personal preference you could use before blocks when

@ul
- There are variables that don't need to be referenced directly but are required
- There are many commands to be executed because its syntax is clearer when many commands are involved
- Creating mocks/stubs
@ulend

---

`Ingredient#==`

```ruby
describe '#==' do
  subject(:pomidor) { described_class(cost: 35) }

  context 'when names and costs are the same' do
    let(:ingredient) { described_class.new('pomidor', 35) }
  
    it 'returns true' do
      expect(ingredient).to eq(pomidor)
    end
  end

  context 'when different names' do
    let(:ingredient) { described_class.new('termidor', 35) }

    it 'returns false' do
      expect(ingredient).not_to eq(pomidor)
    end
  end

  context 'when different costs' do
    let(:ingredient) { described_class.new('pomidor', 10) }

    it 'returns false' do
      expect(ingredient).not_to eq(pomidor)
    end
  end
end
```
@[2]()
@[4-10]()
@[12-18]()
@[20-26]()

+++

What's wrong with these test?

```ruby
describe '#==' do
  first_ingredient = Ingredient.new(name: 'potato', price: 10)
  second_ingredient = Ingredient.new(name: 'potato', price: 10)
  it '==' do
    expect(first_ingredient == second_ingredient).to be_trythy
  end
end
```

+++

How to improve these tests?

```ruby
describe '#==' do
  let(:pomodoro){described_class.new(name: 'Tomato', cost: 100)}
  let(:tomato){described_class.new(name: 'Tomato', cost: 100)}
  context 'when ingredient are equal' do
    it 'they are truly equal' do
      expect(pomodoro).to eq(tomato)
    end
  end
  context 'when ingredient are different' do
    let(:potato){described_class.new(name: 'Potato', cost: 90)}
    it 'they are not equal' do
      expect(potato).not_to eq(tomato)
    end
  end
end
```
@[2]()

---

`IngredientQuantity#total_cost`<br>
@size[0.5em](if cost - per 1 kg; quantity - in grams)

```ruby
let(:tomato) { Ingredient.new(name: 'Tomato', cost: 10) }
subject(:tomatoes_bag) { described_class.new(tomato, quantity: 250) }
let(:potato) { Ingredient.new(name: 'Potato', cost: 5) }
subject(:potatoes_bag) { described_class.new(potato, quantity: 1500) }
  
describe '#total_cost' do
  it 'returns cost of ingredient in quantity' do
    expect(tomatoes_bag.total_cost).to be_within(0.001).of(2.5)
    expect(potatoes_bag.total_cost).to be_within(0.001).of(7.5)
  end
end
```

+++

Improved version

```ruby
subject(:tomatoes_bag) { bag_of(tomato(cost: 10), quantity: 250) }
subject(:potatoes_bag) { bag_of(potato(cost: 5), quantity: 1500) }
  
describe '#total_cost' do
  it 'returns cost of ingredient in quantity' do
    expect(tomatoes_bag.total_cost).to be_within(0.001).of(2.5)
    expect(potatoes_bag.total_cost).to be_within(0.001).of(7.5)
  end
end
```

+++

How it works<br> You can provide some helper for example

```ruby
module Helpers
  def tomato(cost:)
    Ingredient.new('tomato', cost)
  end

  def potato(cost:)
    Ingredient.new('potatoe', cost)
  end

  ...

  def bag_of(ingredient, quantity:)
    IngredientQuantity.new(ingredient, quantity)
  end
end
```

+++

Add file with configurations

```ruby
require 'helpers'

RSpec.configure do |config|
  config.include Helpers
end
```

and "require" it in every file with tests

---

`IngredientQuantity#+`

```ruby
subject(:tomatoes_bag) { bag_of(tomato(cost: 10), quantity: 250) }

describe '#+' do
  context 'when objects with the same ingredient' do
    let(:other_tomatoes_bag) { bag_of(tomato(cost: 10), quantity: 350) }

    it 'returns new IngredientQuantity' do
      summary_bag = tomatoes_bag + other_tomatoes_bag
      expect(summary_bag.quantity).to eq(600)
    end
  end

  context 'when objects with different ingredients' do
    let(:potatoes_bag) { bag_of(potato(cost: 5), quantity: 1500) }
  
    it 'raises error' do
      expect { tomatoes_bag + potatoes_bag }.to raise_error(ArgumentError)
    end
  end
end
```
@[1]()
@[4-11]()
@[13-19]()

+++

What's not good in this code?

```ruby
describe '+' do
  let(:quantity_1) { described_class.new(Ingredient.new(name: 'Tomato', cost: 100), 100) }
  let(:quantity_2) { described_class.new(Ingredient.new(name: 'Tomato', cost: 100), 150) }
  context 'when quantity_1 + quantity_2 with equal ingredients' do
    it 'return new IngredientQuantity' do
      result = described_class.new(Ingredient.new(name: 'Tomato', cost: 100), 250)
      expect(quantity_1 + quantity_2).to eq(result)
    end
  end
  context 'when + quantities with different ingredient' do
    ingredient = Ingredient.new(name: 'Potato', cost: 100)
    let(:potato) { described_class.new(ingredient, 150) }
    it 'they ingredient are not equal' do
      expect { quantity_2 + potato }.to raise_error('arguments are not equal')
    end
  end
end
```
@[11]()

+++

What's bad?

```ruby
before do
  @onion = Ingredient.new(name: 'Onion', cost: 65)
  @another_tomatoes = described_class.new(tomato, quantity: 254)
  @onions = described_class.new(@onion, quantity: 100)
end

describe '#+' do
  context 'when ingredients are same' do
    it 'return object with changes' do
      sum = tomatoes.total_cost + @another_tomatoes.total_cost
      expect((tomatoes + @another_tomatoes).total_cost).to eq(sum)
    end
  end
  context 'when ingredients are not same' do
    it 'return fails' do
      expect(tomatoes + @onions).to raise_error("ingredients are not same")
    end
  end
end
```
@[1-5]()

---

`Recipe#total_cost`

```ruby
let(:tomatoes_bag) { bag_of(tomato(cost: 50), quantity: 250) }
let(:potatoes_bag) { bag_of(potato(cost: 100), quantity: 500) }
subject(:recipe) { Recipe.new('soup', 2, [tomatoes_bag, potatoes_bag]) }

describe '#total_cost' do
  it 'returns cost of the recipe' do
    expect(recipe.total_cost).to be_within(0.001).of(62.5)

    recipe.servings_count = 1
    expect(recipe.total_cost).to be_within(0.001).of(62.5)

    recipe.ingredient_quantities.pop
    expect(recipe.total_cost).to be_within(0.001).of(12.5)
  end
end
```

+++

What's wrong?

```ruby
before(:each) do
  @arr = []
  @arr.push(IngredientQuantity.new(Ingredient.new(name: 'Tomato', cost: 100), 100))
  @arr.push(IngredientQuantity.new(Ingredient.new(name: 'Potato', cost: 50), 100))
  hash = { name: 'borsch', serving_count: 5 }
  @recipe = Recipe.new(hash, @arr)
end
describe '.total_cost' do
  context 'return total cost of recipe' do
    it { expect(@recipe.total_cost).to eq(15) }
    it do
      ingredient = IngredientQuantity.new(Ingredient.new(name: 'Potato', cost: 100), 150)
      @recipe.ingredient_quantities = @recipe.ingredient_quantities.push(ingredient)
      expect(@recipe.total_cost).to eq(30)
    end
  end
end
```
@[1-7]()
@[8]()
@[9]()
@[4,12]()

---

Any other questions?
