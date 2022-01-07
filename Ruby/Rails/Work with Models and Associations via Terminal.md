To create a model we can use the generator: `bin/rails g model User name:string email:string`. 

In OddLand we use Docker so rememeber to open a bash into the container the `rails` command is under `bin/rails` and to create a model you have to type: 

This creates a table in your database that looks like this
```ruby
create_table "users", id: :uuid, default: -> { "gen_random_uuid()" }, force: :cascade do |t|
    t.string "name"
    t.string "email"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end
```
> Note that we use the `uuid` gem here and that the [starting boilerplate]() is already configured to use it.

With this in place we have an empty model that runs the show but since we work in Rails we already have many utilities coming from `ApplicationRecord`:
```ruby
class User < ApplicationRecord
end
```
Once you have your model, like the one above, you're ready to fire a `bin/rails c` and create some records.

You can create a record in multiple steps with `new`:
```bash
user_1 = User.new
user_1.name = 'Andrea'
user_1.email = 'a.barghigiani@gmail.com'
user_1.save # This stores the new User object to the database
```
Or you can create with a one-liner with `create`:
```bash
user_2 = User.create(name: 'Paola', email: 'degiovanni@gmail.com')
```
> We store the returning object of `save` (ran behind the scenes by `create`) to `user_2` but just for using it on the fly.