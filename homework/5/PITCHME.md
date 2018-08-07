# HOMEWORK 4
### Models Pt.2

---

There should be a callback that removes extra spaces in titles of cards, lists
and dashboards.

+++

```ruby
module TitleValidations
  extend ActiveSupport::Concern

  included do
    validates :title, presence: true, length: { maximum: 100 }

    before_validation :remove_extra_spaces

    def remove_extra_spaces
      self.title = title.squish if title.present?
    end
  end
end

class Card < ApplicationRecord
  include TitleValidations
end
```
@[1-13]()
@[15-17]()

---

Returns cards that have labels with the following ids

```ruby
scope :by_labels, ->(label_ids) {
  joins(:labels).where(labels: { id: label_ids })
}
```

+++

Questions

scope :by_labels, <br>
->(label_ids) { joins(:labels).where(labels: { id: label_ids }) }

```ruby
Card.by_labels([1,2,3])

Card.by_labels([])

Card.by_labels(1)

Card.by_labels(1, 2)

labels = Label.find([1,2])
Card.by_labels(labels)
```
@[1](What will return?)
@[3](What will return?)
@[5](What will return?)
@[7](What will return?)
@[9-10](What will return?)

---

Return cards with titles that contain the string

```ruby
scope :by_title, ->(str) {
  where(arel_table[:title].matches("%#{str}%"))
}
```

---

Return cards that should be completed until some time (datetime)
but not completed yet (should be added additional column to track this)

+++

```ruby
scope :should_be_done_until, ->(datetime) {
  where(arel_table[:due_date].lt(datetime)
       .and(arel_table[:completed].eq(false)))
}
```

```ruby
scope :should_be_done_until, ->(datetime) {
  where(arel_table[:due_date].lt(datetime)
       .and(arel_table[:completed_at].eq(nil)))
}
```

---

Return cards that should be completed until current time but not completed yet

```ruby
scope :overdue, -> { should_be_done_until(Time.now) }
```

---

Return cards that don’t have due_date

```ruby
scope :without_due_date, -> { where(due_date: nil) }
```

---

Return dashboards list ordered by title (from A to Z)

```ruby
scope :ordered_by_title, -> { order(:title) }
```

---

Add counter cache column to cards (for comments count)

+++

#### Counter Cache

The **card** model has many **comments**.

+++

#### Counter Cache

In a view we can have something like this:

```erb
<% cards.each do |card| %>
  <h4> <%= card.title %> </h4>
  <%= "#{card.comments.count} comments" %>
<% end %>
```

+++

#### Counter Cache

```sql
SELECT "cards".* FROM "cards"
 (0.2ms)  SELECT COUNT(*) FROM "comments" WHERE "comments"."card_id" = ?  [["card_id", 1]]
 (0.1ms)  SELECT COUNT(*) FROM "comments" WHERE "comments"."card_id" = ?  [["card_id", 2]]
 (0.1ms)  SELECT COUNT(*) FROM "comments" WHERE "comments"."card_id" = ?  [["card_id", 3]]
 (0.1ms)  SELECT COUNT(*) FROM "comments" WHERE "comments"."card_id" = ?  [["card_id", 4]]
```

+++

#### Counter Cache

```ruby
class AddCommentsCount < ActiveRecord::Migration[5.0]
  def change
    add_column :cards, :comments_count, :integer, default: 0
  end
end
```

+++

#### Counter Cache

```ruby
  belongs_to :card, counter_cache: true
```

+++

#### Counter Cache

```erb
<% cards.each do |card| %>
  <h4> <%= card.title %> </h4>
  <%= "#{card.comments_count} comments" %>
<% end %>
```

```sql
SELECT "cards".* FROM "cards"
```

---

Provide scope to order dashboards by most recently updated. It means for example if someone just created new card in some board, that board was updated.

+++

```ruby
class List < ApplicationRecord
  belongs_to :dashboard, touch: true
end

class Card < ApplicationRecord
  belongs_to :list, required: true, touch: true
end
```

```ruby
  scope :ordered_by_updated_at, -> { order(:updated_at) }
```

+++

Read about options of belongs_to<br>
https://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html

---

Any questions?

---
