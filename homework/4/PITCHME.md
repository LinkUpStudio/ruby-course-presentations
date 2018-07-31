# HOMEWORK 4
### Models Pt.1

---

#### Dashboard (Board)

```ruby
class Dashboard < ApplicationRecord
  has_many :lists
  has_many :memberships
  has_many :users, through: :memberships
  belongs_to :user

  validates :user, presence: true
  validates_associated :lists, :users

  validates :title, length: { in: 5..30 }
end
```
@[2-3]

+++

#### Dashboard (Board)

```ruby
class Dashboard < ApplicationRecord
  has_many :lists, dependent: :destroy
  has_many :memberships, dependent: :destroy
  has_many :users, through: :memberships
  belongs_to :user

  validates :user, presence: true
  validates_associated :lists, :users

  validates :title, length: { in: 5..30 }
end
```
@[5,7]

+++

#### Dashboard (Board)

```ruby
class Dashboard < ApplicationRecord
  has_many :lists, dependent: :destroy
  has_many :memberships, dependent: :destroy
  has_many :users, through: :memberships
  belongs_to :user, required: true

  validates_associated :lists, :users

  validates :title, length: { in: 5..30 }
end
```
@[7]

+++

#### Dashboard (Board)

```ruby
class Dashboard < ApplicationRecord
  has_many :lists, dependent: :destroy
  has_many :memberships, dependent: :destroy
  has_many :users, through: :memberships
  belongs_to :owner, class_name: 'User', foreign_key: :user_id, required: true
  # Or if column name is "owner_id":
  # belongs_to :owner, class_name: 'User', required: true

  validates :title, length: { in: 5..30 }
end
```
@[5-7]

---

#### User

```ruby
class User < ApplicationRecord
  has_many :dashboards
  has_many :memberships
  has_many :dashboards, through: :memberships
  has_many :assigned_users
  has_many :cards, through: :assigned_users

  validates_associated :cards, :dashboards, :memberships

  VALID_EMAIL_REGEX = /\A([\w+\-].?)+@[a-z\d\-]+(\.[a-z]+)*\.[a-z]+\z/i

  validates :email, presence: true, format: {with: VALID_EMAIL_REGEX}, uniqueness: true
  validates :username, :password, :email, presence: true
  validates :password, length: {minimum: 10, maximum: 25}
  validates :username, uniqueness: { case_sensitive: true }, length: { in: 5..50, too_long: "%{count} characters is the maximum allowed" }
end
```
@[2,4]

+++

#### User

```ruby
class User < ApplicationRecord
  has_many :owned_dashboards, class_name: 'Dashboard', 
                              foreign_key: :owner_id, 
                              dependent: :nullify
  has_many :memberships
  has_many :dashboards, through: :memberships
  has_many :assigned_users
  has_many :cards, through: :assigned_users

  validates_associated :cards, :dashboards, :memberships

  VALID_EMAIL_REGEX = /\A([\w+\-].?)+@[a-z\d\-]+(\.[a-z]+)*\.[a-z]+\z/i

  validates :email, presence: true, format: {with: VALID_EMAIL_REGEX}, uniqueness: true
  validates :username, :password, :email, presence: true
  validates :password, length: {minimum: 10, maximum: 25}
  validates :username, uniqueness: { case_sensitive: true }, length: { in: 5..50, too_long: "%{count} characters is the maximum allowed" }
end
```
@[5,7]

+++

#### User

```ruby
class User < ApplicationRecord
  has_many :owned_dashboards, class_name: 'Dashboard', 
                              foreign_key: :owner_id, 
                              dependent: :nullify
  has_many :memberships, dependent: :destroy
  has_many :dashboards, through: :memberships
  has_many :assigned_users, dependent: :destroy
  has_many :cards, through: :assigned_users

  validates_associated :cards, :dashboards, :memberships

  VALID_EMAIL_REGEX = /\A([\w+\-].?)+@[a-z\d\-]+(\.[a-z]+)*\.[a-z]+\z/i

  validates :email, presence: true, format: {with: VALID_EMAIL_REGEX}, uniqueness: true
  validates :username, :password, :email, presence: true
  validates :password, length: {minimum: 10, maximum: 25}
  validates :username, uniqueness: { case_sensitive: true }, length: { in: 5..50, too_long: "%{count} characters is the maximum allowed" }
end
```
@[10](We don't need it)
@[14,16](Spaces)
@[14,17](Too long lines)

+++

#### User

```ruby
class User < ApplicationRecord
  has_many :owned_dashboards, class_name: 'Dashboard', 
                              foreign_key: :owner_id, 
                              dependent: :nullify
  has_many :memberships, dependent: :destroy
  has_many :dashboards, through: :memberships
  has_many :assigned_users, dependent: :destroy
  has_many :cards, through: :assigned_users

  VALID_EMAIL_REGEX = /\A([\w+\-].?)+@[a-z\d\-]+(\.[a-z]+)*\.[a-z]+\z/i

  validates :email, presence: true, 
                    format: { with: VALID_EMAIL_REGEX }, 
                    uniqueness: true
  validates :username, :password, :email, presence: true
  validates :password, length: { minimum: 10, maximum: 25 }
  validates :username, uniqueness: { case_sensitive: true }, 
                       length: { 
                         in: 5..50, 
                         too_long: "%{count} characters is the maximum allowed" 
                       }
end
```

---

#### Card

```ruby
class Card < ApplicationRecord
  belongs_to :list
  has_many :assigned_users
  has_many :users, through: :assigned_users
  has_many :comments
  has_many :attachments
  has_many :card_labels
  has_many :labels, through: :card_labels

  validates :list, presence: true
  validates_associated :labels, :users, :comments, :attachments

  validates :title, presence: true, length: { maximum: 50 }
  validates :description, length: { maximum: 1_000 }
end
```

+++

#### Card

```ruby
class Card < ApplicationRecord
  belongs_to :list, required: true
  has_many :assigned_users, dependent: :destroy
  has_many :users, through: :assigned_users
  has_many :comments, dependent: :destroy
  has_many :attachments # We will review it later
  has_many :card_labels, dependent: :destroy
  has_many :labels, through: :card_labels

  validates :title, presence: true, length: { maximum: 50 }
  validates :description, length: { maximum: 1_000 }
end
```

---

#### Assigned user

```ruby
class AssignedUser < ApplicationRecord
  belongs_to :user
  belongs_to :card

  validates :user, presence: true
  validates :card, presence: true
end
```

+++

#### Assigned user

We don't need this model

```ruby
class Card < ApplicationRecord
  has_and_belongs_to_many :users
end
class User < ApplicationRecord
  has_and_belongs_to_many :cards
end
```

---

#### Label

```ruby
class Label < ApplicationRecord
  has_many :card_labels
  has_many :cards, through: :card_labels

  validates_associated :cards
  validates :color, length: { in: 3..15 }
end
```

+++

#### Label

```ruby
class Label < ApplicationRecord
  has_and_belongs_to_many :cards

  enum color: { red: 0, blue: 1, green: 2, yellow: 3 }
end

class Card < ApplicationRecord
  has_and_belongs_to_many :labels
end
```

---

#### Attachment

Wrong

```ruby
class Attachment < ApplicationRecord
  belongs_to :comment
  belongs_to :card

  validates :card, presence: true
  validates :comment, presence: true
end
class Comment < ApplicationRecord
  has_one :attachment
  ...
end
class Card < ApplicationRecord
  has_many :attachments
  ...
end
```

+++

#### Attachment

```ruby
class Attachment < ApplicationRecord
  belongs_to :attachable, polymorphic: true
end

class Card < ApplicationRecord
  has_many :attachments, as: :attachable
end

class Comment < ApplicationRecord
  has_one :attachment, as: :attachable
end
```

---

#### Migrations

```ruby
  def change
    create_table :dashboards do |t|
      t.string :title, null: false
      t.boolean :is_public
      t.belongs_to :user, index: true
      t.timestamps
    end
  end
```
@[4](You should avoid names that start with "is_". https://github.com/rubocop-hq/ruby-style-guide#bool-methods-prefix) 
@[4](Also It's good to have default value here)

+++

#### Migrations

```ruby
  def change
   create_table :users_dashboards, id: false do |t|
      t.belongs_to :user, index: true
      t.belongs_to :dashboard, index: true
   end
  end
```
@[2-5](Rails has better solution for this)

+++

#### Migrations

@size[0.8em](https://apidock.com/rails/ActiveRecord/ConnectionAdapters/SchemaStatements/create_join_table)

```ruby
  def change
    create_join_table :users, :dashboards do |t|
      t.index :user_id
      t.index :dashboard_id
    end
  end
```

---

The end













