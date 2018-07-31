## Ruby on Rails
### @css[no-text-transform](Pt. 3 - A bit more about models)

<!--
sti
seeds -->

---

#### Scopes

More detailed here: <br>
https://guides.rubyonrails.org/active_record_querying.html#scopes

+++

@ul[custom-list]
- Scoping allows you to specify **commonly-used queries** which can be referenced as **method calls on the association objects or models**.
- You can use every method previously covered such as `where`, `joins`, `includes`, etc.
- **All** scope methods will return an `ActiveRecord::Relation` object which will allow for **further methods** (such as other scopes) to be called on it.
@ulend

+++

Simple scope

```ruby
class Article < ApplicationRecord
  scope :published, -> { where(published: true) }
end
```

Totally same, but as method
```ruby
def self.published
  where(published: true)
end
```

+++

Scopes are chainable (within scopes)

```ruby
class Article < ApplicationRecord
  scope :published, -> { where(published: true) }
  scope :published_and_commented,
        -> { published.where('comments_count > 0') }
end
```

+++

Scopes are chainable <br>
Final sql will have all conditions joined with `AND`.

```ruby
class Article < ApplicationRecord
  scope :published, -> { where(published: true) }
  scope :commented, -> { where('comments_count > 0') }
end

Article.published.commented
# => [articles which are published and commented]

category = Category.first
category.articles.published
# => [published articles belonging to this category]
```
@[1-4]()
@[6-7]()
@[9-11]()

+++

Scopes with arguments

```ruby
class Article < ApplicationRecord
  scope :created_before, ->(time) { where('created_at < ?', time) }
end

category.articles.created_before(Time.current)
```

And same as method:

```ruby
def self.created_before(time)
  where('created_at < ?', time)
end
```

+++

Conditionals in scopes! (finally a **difference**)

```ruby
class Article < ApplicationRecord
  scope :created_before,
        ->(time) { where('created_at < ?', time) if time.present? }
end
```

The method **IS NOT** same!

```ruby
def self.created_before(time)
  where("created_at < ?", time) if time.present?
end
```

Scope **always** returns `ActiveRecord::Relation`, while this method could
return `nil`.

---

#### Concerns

More detailed here: <br>
https://api.rubyonrails.org/v5.2/classes/ActiveSupport/Concern.html

+++

Typical module

```ruby
module M
  def self.included(base)
    base.extend ClassMethods
    base.class_eval do
      scope :disabled, -> { where(disabled: true) }
    end
  end

  module ClassMethods
    # ...
  end
end
```
@[2-7]()
@[9-11]()

+++

Same module with `ActiveSupport::Concern`

```ruby
require 'active_support/concern'

module M
  extend ActiveSupport::Concern

  included do
    scope :disabled, -> { where(disabled: true) }
  end

  class_methods do
    # ...
  end
end
```
@[1]()
@[4]()
@[6-8]()
@[10-12]()

+++

Real world example

```ruby
module Discardable
  extend ActiveSupport::Concern

  included do
    scope :discarded, -> { where.not(discarded_at: nil) }
    scope :kept, -> { where(discarded_at: nil) }
  end

  def discard
    update_attribute(discarded_at: Time.current)
  end

  def kept?
    !discarded_at
  end

  def discarded?
    !kept?
  end
end
```
@[4-7]()
@[9-11]()
@[13-15]()
@[17-19]()
