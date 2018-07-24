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

More detailed about migrations you can find here: <br> https://edgeguides.rubyonrails.org/active_record_migrations.html

---

#### Associations


