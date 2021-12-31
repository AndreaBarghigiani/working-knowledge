> Notes taken while reading [Learn Ruby on Rails Best Practices With One Exercise](https://www.hexdevs.com/posts/learn-ruby-best-practices-with-one-exercise/)

# Code review
We reviwed [`practice.rb`](https://github.com/thoughtbot/upcase/blob/master/app/services/practice.rb) and we get asked the following questions.

**Why this class is so short?**
Because we rely on ActiveRecords methods to query our database. Also because in the methods we use, where needed, we use shorthands to make our code shorter. The `&` in `trails.select(&:just_finished?)` tells me that we're passing the itereted elemento to `just_finished?` Active Record method, but need to be sure about.

**Why is the variable `trails` beign passed down to `initialize` constructor?**
Well tbh I don't know yet...

**What is the meaning of `promoted_unstarred_trails` and `unpromoted_unstarred_trails`?**
From what I understand the first add the trails in the *favorite* and the second one removes it from. Don't know why they've that names

**What is the class responsible for?**
From my point of view the class has mixed behavior inside, it checks if the studend has completed tracks to go to the favorites functionalities. 