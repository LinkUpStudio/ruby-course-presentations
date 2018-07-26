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

#### Schema Conventions

@ul[custom-list]
- **Foreign keys** -
  should be named following the pattern **singularized_table_name_id**
  (e.g., item_id, order_id)
- **Primary keys** -
  by default, Active Record will use an integer column named **id** as the
  table's primary key
@ulend

---

#### Migrations

A **migration** is a set of database instructions. <br>
Those instructions are Ruby code, which migrates our database from one state to
another.
Essentially they describe database changes.

+++

#### Migrations

Each migration is a separate file, which Rails runs for us when we instruct it.
Rails keeps track of what's been run, so changes don't get attempted more than
once.

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
@[4](with "email" column)
@[6](and with timestamps fields "created_at" and "updated_at")
@[3-7](primary key will be automatically added (as "id" column))

+++

#### Migrations

@size[0.8em](In some cases you have to use `up` and `down` instead of `change`)

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
@[2-6](called on `rails db:migrate`)
@[8-12](called on `rails db:rollback`)

@size[0.8em](But when writing constructive migrations - adding tables or columns, always use the `change` method)

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
@[4](Column type is needed for automatic rollback of this migration)

+++

#### Most useful options for columns

@ul[custom-list]
- `null` - allows or disallows NULL values in the column
- `default` - allows to set a default value on the column
- `index` - adds an index for the column
@ulend

+++

#### Another example of migration

```ruby
change_table :users do |t|
  t.integer :reviews_count, default: 0, null: false

  t.belongs_to :city
end

add_index :users, :email
```
@[2](Add/change if exists the "reviews_count" column that cannot be null and has a default value)
@[4](Add **indexed** foreign key "city_id")
@[7](Add index to "email" column of "users" table)

+++

#### Running and Rolling Back Migrations

```bash
$ rails db:migrate

$ rails db:migrate VERSION=20080906120000

$ rails db:rollback

$ rails db:rollback STEP=3

$ rails db:setup

$ rails db:drop

$ rails db:reset
```
@[1](perform all migrations from current version to latest)
@[3](migrate OR rollback to specified version)
@[5](rollback to previous version)
@[7](rollback given amount of migrations)
@[9](runs `db:create` ~db:migrate~ `db:schema:load` `db:seed`)
@[11](DESTROYS your whole db (ALSO, `db:schema:load` **cleans up all your DATA**))
@[13](runs `db:drop` and `db:setup`)

+++

#### Migrations

More details about migrations you can find here: <br>
https://edgeguides.rubyonrails.org/active_record_migrations.html

---

#### CRUD: Initialization

```ruby
user = User.new
#=> #<User id: nil, email: nil, name: nil, created_at: nil, updated_at: nil>
user.name = 'John'
#=> "John"
user.email = 'john@kamman.com'
#=> "john@kamman.com"

user = User.new(name: 'John', email: 'john@kamman.com')
#=> #<User id: nil, email: "john@kamman.com", name: "John", created_at: nil, updated_at: nil>

user = User.new do |u|
  u.name = 'John'
  u.email = 'john@kamman.com'
end
#=> #<User id: nil, email: "john@kamman.com", name: "John", created_at: nil, updated_at: nil>

```
@[1-6]()
@[8-9]()
@[11-15]()

+++

#### CRUD: Creation

```ruby
user = User.create(name: 'John', email: 'john@kamman.com')
#=> #<User id: 1, email: "john@kamman.com", name: "John", created_at: "2018-07-25 17:11:03", updated_at: "2018-07-25 17:11:03">

user = User.new(name: 'John', email: 'john@kamman.com')
#=> #<User id: nil, email: "john@kamman.com", name: "John", created_at: nil, updated_at: nil>
user.save
#=> true
```
@[1-2]()
@[4-7]()

+++

#### CRUD: Reading

```ruby
users = User.all
#=> #<ActiveRecord::Relation [#<User id: 1, email: "john@kamman.com", name: "John", created_at: "2018-07-25 17:11:03", updated_at: "2018-07-25 17:11:03">]>

user = User.first
#=> #<User id: 1, email: "john@kamman.com", name: "John", created_at: "2018-07-25 17:11:03", updated_at: "2018-07-25 17:11:03">

user = User.find(1)
#=> #<User id: 1, email: "john@kamman.com", name: "John", created_at: "2018-07-25 17:11:03", updated_at: "2018-07-25 17:11:03">

user = User.find(4)
# ActiveRecord::RecordNotFound (Couldn't find User with 'id'=4)

user = User.find_by(id: 4)
#=> nil

user = User.find_by(email: 'john@kamman.com')
#=> #<User id: 1, email: "john@kamman.com", name: "John", created_at: "2018-07-25 17:11:03", updated_at: "2018-07-25 17:11:03">

users = User.where(name: 'John')
#=> #<ActiveRecord::Relation [#<User id: 1, email: "john@kamman.com", name: "John", created_at: "2018-07-25 17:11:03", updated_at: "2018-07-25 17:11:03">]>
```
@[1-2]()
@[4-5]()
@[7-8]()
@[10-11]()
@[13-14]()
@[16-17]()
@[19-20]()

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
@[1-3]()
@[5-6]()
@[8](single DB query, no ruby objects, no validations/callbacks)

+++

#### CRUD: Deleting

```ruby
user = User.find(4)
user.destroy

User.where(name: 'John').destroy_all

User.destroy_all

User.delete_all
```
@[1-2]()
@[4]()
@[6]()
@[8](single DB query, no ruby objects, no callbacks)

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
@[1-3](Name should be present)
@[5-6]()
@[8-9]()
@[11-14]()
@[16-19]()
@[21-24]()
@[26-27]()
@[29-30]()
@[32-33]()

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

Read more here: <br>
https://guides.rubyonrails.org/active_record_validations.html#validation-helpers

+++

#### Custom method for validation

```ruby
class Invoice < ApplicationRecord
  validate :customer_is_active, on: :create

  private

  def customer_is_active
    return if customer.active?
    errors.add(:customer, 'customer must be active')
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

  before_validation :ensure_name_has_value

  private

  def ensure_name_has_value
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

#### `belongs_to`

A belongs_to association sets up a one-to-one connection with another model,
such that each instance of the declaring model "belongs to" one instance of the
other model.

+++

#### `has_many`

A has_many association indicates a one-to-many connection with another model.
You'll often find this association on the "other side" of a belongs_to
association. This association indicates that each instance of the model has
zero or more instances of another model.

+++

#### `belongs_to` and `has_many`

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

#### `has_one`

A has_one association also sets up a one-to-one connection with another model,
but with somewhat different semantics (and consequences). This association
indicates that each instance of a model contains or possesses one instance of
another model.

+++

#### `has_one`

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

#### `has_many :through`

First example of usage (one user could be a member of many teams)

```ruby
class Team < ApplicationRecord
  has_many :memberships
  has_many :users, through: :memberships
end

class User < ApplicationRecord
  has_many :memberships
  has_many :teams, through: :memberships
end

class Membership < ApplicationRecord
  belongs_to :user
  belongs_to :team
  # and has some additional data, like user's role in a team
end
```

+++

#### `has_many :through`

Another example of usage (one user could be a member of only 1 team)

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
@[13](unusual, but cool usage of `has_one`)

+++

#### `has_and_belongs_to_many`

No intervening model, but join table "teams_users" is still required

```ruby
class Team < ApplicationRecord
  has_and_belongs_to_many :users
end

class User < ApplicationRecord
  has_and_belongs_to_many :teams
end
```

+++

#### `has_and_belongs_to_many`

You should use `has_many :through` if you need validations, callbacks or extra
attributes on the join model.

Also `has_many :through` is considered more flexible and "ready for future".

+++

#### Polymorphic Association

```ruby
class Company < ApplicationRecord
  has_many :pictures, as: :imageable
end

class User < ApplicationRecord
  has_many :pictures, as: :imageable
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

+++

##### Self study

Read more about associations: <br>
https://guides.rubyonrails.org/association_basics.html<br>
Especially about different **options** for `belongs_to`, `has_many`, etc.

---

let's take a small break here

---

#### Enum

Declare an enum attribute where the values map to integers in the database, but can be queried by name

```ruby
class Payment < ActiveRecord::Base
  enum status: { pending: 0, success: 1, denied: 2 }
end
```

```ruby
create_table :payments do |t|
  t.column :status, :integer, default: 0
end
```

+++

#### Enum

```ruby
payment = Payment.new
payment.status   # => "pending"
payment.success!
payment.success? # => true
payment.status   # => "success"

Payment.success
Payment.denied

Payment.where(status: [:pending, :success])
```
@[1-5]()
@[7-8](Scopes based on the allowed values of the enum field will be provided)
@[10](You can also query them directly)

+++

#### Enum

@size[0.8em](Also you can read about `_suffix` and `_prefix` options here: https://api.rubyonrails.org/v5.2.0/classes/ActiveRecord/Enum.html)
@size[0.8em](And about gem that will help you to realize enum with multiple values: https://github.com/brainspec/enumerize)

---

more about queries, options in associations, STI, counter_cache, concerns? n+1 problem...
