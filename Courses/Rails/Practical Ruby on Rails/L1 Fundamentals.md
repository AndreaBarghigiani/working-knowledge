## Request/Response  Cycle
The browser makes a request that can be `GET`, `POST`, `PUT` (or `PATCH`), `DELETE`, `OPTIONS`.

The `PATH` that a user request is the part after the domain but before the query params. In the `routes.rb` we have most  of this informations:

![[CleanShot 2022-08-03 at 08.19.03.jpg]]

# The Database: keep state around
In the context of Rails apps a database is a different application the you connect to to access to the application state.

The relational database uses tables that holds data inside rows and columns. For each resource we will have **at least** one table. A resource has fields that are organized into columns and each resorce will be stored into a row.

Tables can be related into each other.

## Migrations
This is the tool to create/modify tables. `rails db:migrate` is the CLI command that allows you to run all the pending migrations.

We can talk to a database using Rails code instead of SQL with `rails console`. To list all the rows in the `users` table all we have to do is to write `User.all`. If you want to get into the standard SQL database console `rails db` is the command you're looking for.

To know the database structure of the project go over all migrations file is time consuming, there's a better option though and it is the `db/schema.rb`.

Is possible to add testing data into an application with the `db/seed.rb`, you could add each entry by hand via che `create` method in `rails console` but you're gonna waste a bunch of time and it's difficult to share the data you're working on. With a `seed.rb` file you can **write the same Rails command** and store them into a file, all you have to do after you set up the data is to run the `rails db:seed` command to import those.

### Naming Conventions
Rails is 'smart' enough to know what you want to do with your database by following some naming conventions. Related to databases the most important one is how it uses singular and plurals.

For example you maybe think to create a User model the migration will create a `users` table and a set of common methods that we can use inside our app.

![[CleanShot 2022-08-13 at 07.37.23.jpg]]

We need to **pay attention** to singular and plurals becase as you can see from the picture, depending on the view we want methods and paths can change.

### Indexing
Since the loading of a web page is just a back and forth between the client and the server, where the first is asking for some data to be displayed while the second will check the database for those.

Indexing some fields **makes those searches go faster** by creating a new table where we keep just the most common data we search for.

We can see the indexes in the `schema.rb` file.

### Relationships
When we talk about database design is a good idea to make our tables **atomic**, it means they should be *'one thing'*. For example a `full_name` is the join of `first_name` and `last_name`.

A relational database uses the relationships between table to find the information we're looking for.

A **key** makes the relationship between table possible, generally we use IDs to make the search faster

### CRUD
*Create Read Update Delete* and acronym highly used in a Rails application