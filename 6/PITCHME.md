## Ruby on Rails
### @css[no-text-transform](Pt. 2 - Models)

---

#### ORM (Object-Relational Mapping)

technique that connects the **rich objects** of an application to **tables** in a RDBMS. <br>

@size[0.7em](The properties and relationships of the objects in an application can be easily stored and retrieved from a database without writing SQL statements directly and with less overall database access code.)

+++

#### Active Record as an ORM Framework

@ul[custom-list]
- Represent models and their data
- Represent associations between these models
- Represent inheritance hierarchies through related models
- Validate models before they get persisted to the database
- Perform database operations in an object-oriented fashion
@ulend

+++

#### Naming Conventions

@size[1em](**DB Table** - plural with underscores separating words) <br>
@size[1em](**Model** - singular with the first letter of each word capitalized) <br>

@size[0.8em](Model: `Article`, table: `articles`)<br>
@size[0.8em](Model: `LineItem`, table: `line_items`)<br>
@size[0.8em](Model: `Deer`, table: `deers`)<br>
@size[0.8em](Model: `Mouse`, table: `mice`)<br>
@size[0.8em](Model: `Person`, table: `people`)

+++

#### Schema Conventions ??????

@ul[custom-list]
- **Foreign keys** - should be named following the pattern **singularized_table_name_id** (e.g., item_id, order_id)
- **Primary keys** - by default, Active Record will use an integer column named **id** as the table's primary key
@ulend

---

#### Migrations

A **migration** is a set of database instructions. <br>
Those instructions are Ruby code, which migrates our database from one state to another. 
Essentially they describe database changes.

+++

#### Migrations

Each migration is a separate file, which Rails runs for us when we instruct it. 
Rails keeps track of what's been run, so changes don't get attempted more than once.

+++

#### Example of migration

```ruby
class CreateUsers < ActiveRecord::Migration[5.0]
  def change
    create_table :users do |t|
      t.string :email
 
      t.timestamps
    end
  end
end
```
@[3](Creates "users" table)
@[4-5](with "email" column)
@[7](and with timestamps fields "created_at" and "updated_at")
@[3-7](primary key will be automatically added (as "id" column))

+++

#### Migrations

In some cases you have to use `up` and `down` instead of `change`

```ruby
class ChangeUsersBirthDay < ActiveRecord::Migration[5.0]
  def up
    change_table :users do |t|
      t.change :birth_day, :string
    end
  end

  def down
    change_table :users do |t|
      t.change :birth_day, :date
    end
  end
end
```

But when writing constructive migrations (adding tables or columns), always use the `change` method.

+++

#### Migrations creation

```bash
$ rails g migration AddNameToUsers name:string
```

```ruby
class AddNameToUsers < ActiveRecord::Migration[5.0]
  def change
    add_column :users, :name, :string
  end
end
```

+++

#### Migrations creation

```bash
$ rails g migration RemoveNameFromUsers name:string
```

```ruby
class RemoveNameFromUsers < ActiveRecord::Migration[5.0]
  def change
    remove_column :users, :name, :string
  end
end
```
@[3](Type of column need it to rollback this migration)

+++

#### Most useful options for columns

@ul[custom-list]
- `null` - allows or disallows NULL values in the column
- `default` - allows to set a default value on the column
- `index` - adds an index for the column
@ulend

#### Another example of migration

```ruby
change_table :users do |t|
  t.integer :reviews_count, default: 0, null: false

  t.belongs_to :city
end

add_index :users, :email
```
@[2](Add/change if exists the "reviews_count" column that cannot be null and has a default value)
@[4](Add indexed foreign key "city_id")
@[7](Add index to "email" column of "users" table)

+++

#### Migrations

More details about migrations you can find here: <br> https://edgeguides.rubyonrails.org/active_record_migrations.html

---

#### CRUD: Initialization

```ruby
user = User.new
user.name = 'John'
user.email = 'john@kamman.com'

user = User.new(name: 'John', email: 'john@kamman.com')

user = User.new do |u|
  u.name = 'John'
  u.email = 'john@kamman.com'
end
```

+++

#### CRUD: Creation

```ruby
user = User.create(name: 'John', email: 'john@kamman.com')

user = User.new(name: 'John', email: 'john@kamman.com')
user.save
```

+++

#### CRUD: Reading

```ruby
users = User.all

user = User.first

user = User.find(4)

user = User.find_by(email: 'john@kamman.com')

users = User.where(name: 'John')
```

+++

#### CRUD: Updating

```ruby
user = User.find_by(email: 'john@kamman.com')
user.email = 'john.kamman@gmail.com'
user.save

user = User.find_by(email: 'john@kamman.com')
user.update(email: 'john.kamman@gmail.com')

User.update_all(email_confirmed_at: nil)
```

+++

#### CRUD: Deleting

```ruby
user = User.find(4)
user.destroy

User.where(name: 'John').destroy_all

User.destroy_all
```

---

#### Validations

```ruby
class User < ApplicationRecord
  validates :name, presence: true
end

User.new(name: 'John Doe').valid? 
#=> true

User.new.valid? 
#=> false

user = User.new
#=> #<User id: nil, name: nil>
user.errors.messages
#=> {}

user.valid?
#=> false
user.errors.messages
#=> {name:["can't be blank"]}

user = User.create
#=> #<User id: nil, name: nil>
user.errors.messages
#=> {name:["can't be blank"]}

user.save
#=> false

user.save!
#=> ActiveRecord::RecordInvalid: Validation failed: Name can't be blank

User.create!
#=> ActiveRecord::RecordInvalid: Validation failed: Name can't be blank
```

+++

#### Main validation helpers

```ruby
validates :name, :email, presence: true
```

```ruby
validates :email, uniqueness: true
```

```ruby
validates :sing_in_count, numericality: { only_integer: true }
```

Read more here: https://guides.rubyonrails.org/active_record_validations.html#validation-helpers

+++

#### Custom method for validation

```ruby
class Invoice < ApplicationRecord
  validate :active_customer, on: :create

  def active_customer
    errors.add(:customer_id, 'is not active') unless customer.active?
  end
end
```

+++

#### Methods that trigger validations

@ul[custom-list]
- `create`
- `create!`
- `save`
- `save!`
- `update`
- `update!`
@ulend

+++

#### Skipping Validations

@ul[custom-list]
- `update_all`
- `update_attribute`
- `update_column`
- `update_columns`
- `save(validate: false)`
- else (https://guides.rubyonrails.org/active_record_validations.html#skipping-validations)
@ulend

---

#### Callbacks

```ruby
class User < ApplicationRecord
  validates :name, :email, presence: true

  before_validation :ensure_name_has_a_value

  private

  def ensure_name_has_a_value
    self.name = email[/\w+/] if name.nil? && email
  end
end
```

+++

#### Callbacks

@ul[custom-list]
- `before_validation`, `after_validation`
- `before_save`, `around_save`, `after_save`
- `before_create`, `around_create`, `after_create`
- `before_update`, `around_update`, `after_update`
- `before_destroy`, `around_destroy`, `after_destroy`
- `after_commit`/`after_rollback`
- also `after_initialize`, `after_find` and `after_touch`
@ulend

+++

#### Running Callbacks

@ul[custom-list]
- `create`, `create!`
- `destroy`, `destroy!`, `destroy_all`
- `save`, `save!`, `save(validate: false)`
- `update_attribute`, `update`, `update!`
- `valid?`
- else (https://guides.rubyonrails.org/active_record_callbacks.html#running-callbacks)
@ulend

+++

#### Skipping Callbacks

@ul[custom-list]
- `delete`
- `delete_all`
- `update_column`
- `update_columns`
- `update_all`
- else (https://guides.rubyonrails.org/active_record_callbacks.html#skipping-callbacks)
@ulend

---

#### Associations

In Rails, an association is a connection between two Active Record models

+++

#### Types of Associations

@ul[custom-list]
- belongs_to
- has_one
- has_many
- has_many :through
- has_one :through
- has_and_belongs_to_many
@ulend

+++

#### `belongs_to` Association

A belongs_to association sets up a one-to-one connection with another model, such that each instance of the declaring model "belongs to" one instance of the other model.

+++

#### `has_many` Association

A has_many association indicates a one-to-many connection with another model. You'll often find this association on the "other side" of a belongs_to association. This association indicates that each instance of the model has zero or more instances of another model.

+++

#### `belongs_to` and `has_many` Associations

```ruby
class Team < ApplicationRecord
  has_many :users
end

class User < ApplicationRecord
  belongs_to :team
end
```

In "users" table must be "team_id" column.

+++

#### The `has_one` Association

A has_one association also sets up a one-to-one connection with another model, but with somewhat different semantics (and consequences). This association indicates that each instance of a model contains or possesses one instance of another model. 

+++

#### The `has_one` Association

```ruby
class Passport < ApplicationRecord
  belongs_to :user
end

class User < ApplicationRecord
  has_one :passport
end
```

In "passports" table must be "user_id" column.

+++

#### `has_many :through` Association

First example of usage

```ruby
class Team < ApplicationRecord
  has_many :members
  has_many :users, through: :members
end

class User < ApplicationRecord
  has_many :members
  has_many :teams, through: :members
end

class Member < ApplicationRecord
  belongs_to :user
  belongs_to :team
end
```

+++

#### `has_many :through` Association

Another example of usage

```ruby
class Company < ApplicationRecord
  has_many :teams
  has_many :users, through: :teams
end

class Team < ApplicationRecord
  belongs_to :company
  has_many :users
end

class User < ApplicationRecord
  belongs_to :team
  has_one :company, through: :team
end
```

+++

#### `has_and_belongs_to_many` Association

No intervening model

```ruby
class Team < ApplicationRecord
  has_and_belongs_to_many :users
end

class User < ApplicationRecord
  has_and_belongs_to_many :teams
end
```

Table "teams_users" is required

+++

#### `has_and_belongs_to_many` Association

You should use `has_many :through` if you need validations, callbacks or extra attributes on the join model.

+++

#### Polymorphic Association

```ruby
class Company < ApplicationRecord
  has_many :images, as: :imageable
end

class User < ApplicationRecord
  has_many :images, as: :imageable
end

class Picture < ApplicationRecord
  belongs_to :imageable, polymorphic: true
end
```

+++

#### Polymorphic Association

```ruby
class CreatePictures < ActiveRecord::Migration[5.0]
  def change
    create_table :pictures do |t|
      t.string :name
      t.references :imageable, polymorphic: true, index: true
      t.timestamps
    end
  end
end
```

---

more about queries, options in associations, STI, counter_cache, concerns? n+1 problem...

