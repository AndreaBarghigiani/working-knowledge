We use those any day working in Rails first example of DSL is present in our `config/routes.rb`:
```ruby
Rails.application.routes.draw do
  root to: 'home#index'
  resources :users
end
```
In this example the DSL is present in the `resources` method that allow us to pass under the hood all the definition with request types, path and params, you just pass `:user`.

# Other examples
## Database migrations
In our migrations we simply write Ruby to translate it SQL and the DSL in use allows us to define columns, types, indexes, and many more.
```ruby
class Migration < ActiveRecord::Migration[6.1]
  def change
    add_column :users, :first_name, :string
  end
end
```
## Configurations
```ruby
MyGem.configure do
  config.some_setting = 'value'
end
```