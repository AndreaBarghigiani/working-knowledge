# MVC - Model View Controller
Separates the data from the way it's presented to the user.

![[CleanShot 2022-08-13 at 08.08.17.jpg]]

The controller handle the request, require some data from the model and once retrieved passes the data to the view that it builds the HTML and sends back it to the controller that is then sent as webpage from the controller to the browser.

**Model:**
- contains the data of the app (in DB)
- provides data to the Controller
- handles data related logic
- has no knowledge about the View (or how the data will be used)

**View:**
- has no knowledge about the Model
- builds the page with data from the Controller
- returns the built page (HTML, CSS, JS) to the Controller

**Controller:**
- handles the request from the browser
- fetches data from the Model
- provides data to the View
- returns the responso to the browser

## Models 
Each model in a Rails application talks to databases via Ruby objects via the *Object Relational Mapping (ORM)* implemented via the Active Record that provides:
- Mapping
- Associations
- Validation

When we have a model we get all the behavior that Active Record provides even futher the standard CRUD, for example we can create without write.

It is always better to have **consistend data** because it simplify your code taking out the need to write checks.

We can also write custom methods on the Model and this show us that we are not limited by the CRUD operations.

### Validations
Looks at the data sent before putting them into the database, this improves data consistency.

![[CleanShot 2022-08-27 at 07.37.43.jpg]]

The more consistent is the data the less code you have to write.

#### Confirmations
In some forms we ask twice the presence of a field, email confirmation for example, but we do not want that data into the database (will just be a duplicate) but the validation needs to run the validation on both but Active Record has our back.

All it needs its to add `confirmation: true` to the validation

```ruby
validates :email, presence: true, confirmation: true
```

We will have the `email_confirmation` field that will be taken into account when we run the `valid?` method when it's value is different than `nil`.

#### Format
We can assure that only specific type of data get into the database via the `format` and `length` attributes, the first commonly accepts a Regex via the `with` property of the settings object while the second is able to set different properties for `minimum` and `maximum` string lengths.

Here's [all the docs](https://api.rubyonrails.org/classes/ActiveModel/Validations/HelperMethods.html#method-i-validates_format_of) you'll even need.

### Associations
Those are the relationships inside our database.

#### One-to-One / Has-One & Belongs-to
This produces a 1:1 association where the data of each table are strictly connected. We use the `belongs_to` to the model that has the foreign_key and `has_one` to the model that's connected to.

#### One-to-Many / Has-Many
With Active Record it's pretty simple to add this kind of relationship, following the example if a user can have multiple emails we have to define `has_many` into the model **and** pluralize the reference name and this changes all the methods to check the association. For example instead of `user.email` now we have to use the `user.emails`.

#### Many-to-Many / Has-and-Belongs-to-Many
Those uses a join table to connect the various IDs and this is how to create the migrations for the table itself:
![[CleanShot 2022-08-27 at 07.59.10.jpg]]

Now that we have the table we need to add the association in each model with `has_and_belongs_to_many` keyword.

#### Has-Many-Through
![[CleanShot 2022-08-27 at 08.05.38.jpg]]

This specific case is useful when we need to leverage the join table to store additional data into the database. In the example we have users **orders** that we store and the orders table are a join table that has columns for the **price, quantity and creation date** of the order.

Then we connect the Users and Products tables via the Orders table, thanks to `through` .

#### Polymorphic
Often  used when a model can belong to multiple models, like comments for example. Comments can be shared between blog posts, videos and products so we need to set the join table to accomodate all of those use cases.

![[CleanShot 2022-08-27 at 18.23.19.jpg]]

### Queries
The queries are straight SQL commands used to access and interrogate the database.

#### Finders
`select * from users` it's a common finder query but we can run finding queries right within the Rails console calling the `find` method on the model class and providing the ID for the searched record.

#### Conditions
The `where` method of Active Record allows us to select a specific group of data that share common informations.
```ruby
User.where("name = 'John'") # Standard SQL string
```

Generally speaking writing SQL string is not advisable because it can fire security concerns, a better  way to do it is writing [*positional handlers*](https://guides.rubyonrails.org/security.html#sql-injection-countermeasures) where the question mark is changed with its following attributes:
```ruby
User.where("name = ? AND email = ?", params[:search_term], "email")
# SELECT "users".* FROM "users" WHERE (name = 'John' AND email = 'email')
```

While replacing the strings gets sanitized as well.

Another approach is to use [*placeholder conditions*](https://guides.rubyonrails.org/active_record_querying.html#placeholder-conditions) that looks like this:
```ruby
User.where("name = :name AND email = :email", {name: "John", email: "email"})
```

Or we can simply use the hash syntax:
```ruby
User.where(name: "John")
```

We can also **negate** a query using the `not` method:
```ruby
User.where.not(name: "John")
```

Or combine multiple conditions with a `or` method:
```ruby
User.where(name: "John").or(User.where(name: "John"))
```

#### Ordering
Chaining multiple methods not always means to have multiple SQL query, for example the following query, that retrieve the last users and order them in descending order, creates just a single SQL:
```ruby
User.where("created_at >= :this_month", { this_month: Date.today.beginning_of_month }).order(created_at: desc)`
```

Each of these methods returns an Active Record Relation and the SQL gets generated at the end.

#### Scopes
Scopes are written in the model itself and helps declutter your code, here's one that will retrieve all the active users:
```ruby
scope :active, -> { where(active: true) }
```

And we can use by calling it from the class with `User.active` just like any other method.

We can also write scopes that accepts arguments, let's say  we want to create a scope `older_than`  but we want to pass the age, here's how we should write the scope:
```ruby
scope :older_than, ->(age) { where("age > ?", age) }
```

Now that we have two scopes we can also chain them `User.older_than(25).active`.

#### Joins
Joins are useful when you want to retrieve informations of a table based on the data related to an external table. For example we want to list all the `users` that have selected specific `foods`. 

First and foremost we need to tell Active Record about this association adding `has_many` to the `User` model, then we are able to write a query like this:
```ruby
User.where( foods: { name: "Lettucce" }).joins(:foods)
```

#### Eager Loading
Joins uses *lazy loading* and not always is the correct solution because even in case of providing an array it executes each command one at the time. Eager Loading helps in this with the Eager Loading.

### Callbacks
Active Record provide several methods that allow us to intercept specific events in the lifecycle of a model, and it does that via *callbacks*.

```ruby
class User < ApplicationRecord
  before_validation { puts "before_validation called" }
  before_save       { puts "before_save called" }
  before_create     { puts "before_create called" }

  after_validation  { puts "after_validation called" }
  after_create      { puts "after_create called" }
  after_save        { puts "after_save called" }
  after_commit      { puts "after_commit called" }
  after_rollback    { puts "after_rollback called" }

  before_validation :cleanup

  private

    def cleanup
      name.strip!
    end
end
```

## Views 
They are in charge of generating the HTML that'll be sent to the browser.

### Templates
We sore our templates in `app/views/<controller-name>` as per convention, also each file in that folder will have a dedicated action in the controller and it will be the default view for it.

### Layouts
Wrapper for all (or a part) of our templates, there are other generic section of the page that are part of the layout as the Header and Footer sections.

![[CleanShot 2022-09-04 at 18.34.08.jpg]]

The default layout is `app/views/layouts/application.html.erb`.

### Partials
![[CleanShot 2022-09-04 at 18.36.35.jpg]]

Sections of a page that you can re-use, like a form to create a user. Since it will be the same in the `new` and `edit` page, it is a good idea save it as a partial and use in both. 

![[CleanShot 2022-09-04 at 18.39.06.jpg]]

Any partial that includes a variable needs to be rendered with that value provided via `locals`.

### Forms
To create a form we use the `form_with` that gives us an object full of helpful methods to create the fields we need. But this is also able to generate a form that **represent a Model** 

## Controller
Parameters are used to send data into our controller via forms or URL, with the first the data get wrapped and are available inside the controller via the `params` hash.