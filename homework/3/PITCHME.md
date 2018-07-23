# HOMEWORK 3

---

First of all<br>
It's good to test constructors too

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

+++

What remarks could be for these tests?

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
@size[0.5em](cost - per 1 kg)<br>
@size[0.5em](quantity - in grams)

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

Improved version of `IngredientQuantity#total_cost`

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

How? You can provide some helper for example

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
subject(:potatoes_bag) { bag_of(potato(cost: 5), quantity: 1500) }

describe '#+' do
  context 'when objects with the same ingredient' do
    let(:other_tomatoes_bag) { bag_of(tomato(cost: 10), quantity: 350) }

    it 'returns new IngredientQuantity' do
      summary_bag = tomatoes_bag + other_tomatoes_bag
      expect(summary_bag.quantity).to eq(600)
    end
  end

  context 'when objects with different ingredients' do
    it 'raises error' do
      expect { potatoes_bag + tomatoes_bag }.to raise_error(ArgumentError)
    end
  end
end
```

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
