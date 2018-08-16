## Ruby on Rails
### @css[no-text-transform](Few nice gems)

---

#### @css[no-text-transform](Devise)

https://github.com/plataformatec/devise

one of the most popular :)

**NOTE:** it shouldn't be your choice for API-only apps,
in that case you could use [knock](https://github.com/nsarno/knock) instead.

---

#### @css[no-text-transform](Pundit)

https://github.com/varvet/pundit

must have one. Helps you to manage permissions logic, e.g. **authorization**
(not _authentication_).

[cancancan](https://github.com/CanCanCommunity/cancancan) is an alternative,
but it's a bit too "_magical"_.

---

#### @css[no-text-transform](Bullet)

https://github.com/flyerhzm/bullet

helps you to spot N+1 Query Problems AND unused eager loading.

---

#### @css[no-text-transform](Enumerize)

https://github.com/brainspec/enumerize

basically very similar to built-in ActiveRecord `enum`, but a bit more advanced,
and it has support for "multiple" enums, e.g. like an array of enum values.

---

#### @css[no-text-transform](Kaminari)

https://github.com/kaminari/kaminari

Paginator for Ruby webapps

---

#### @css[no-text-transform](RailsAdmin)

https://github.com/sferik/rails_admin

Easily generates admin panel. In most cases it's just enough.


But when you need something more custom, well... there's also
[Administrate](https://github.com/thoughtbot/administrate), and
[Active Admin](https://github.com/activeadmin/activeadmin)
but maybe you will have to write your own admin panel... (which seems easy but
actually **a lot** of work).

---

#### @css[no-text-transform](CarrierWave)

https://github.com/carrierwaveuploader/carrierwave

this is easy file-management to begin with. For more advanced features you
should use [shrine](https://github.com/shrinerb/shrine)

---

#### @css[no-text-transform](Sidekiq)

https://github.com/mperham/sidekiq

Background processing with job queues stored in [redis](https://redis.io/).

---

#### @css[no-text-transform](dry-transaction)

http://dry-rb.org/gems/dry-transaction/

A great place for your business logic :)
But it could seem a bit hard to grasp for beginners, so you could try
[mutations](https://github.com/cypriss/mutations) instead, they are pretty cool
too, but dry-transaction is more advanced.
