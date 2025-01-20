Each section will be a table into the database.

## User
- `id`
- `username`
- `email`
- `role` - `enum` that will allow the user to get more power 
	- `author`: default and allow the user to create a project
	- `editor`: can edit any project (this will be useful in the future)
	- `subscriber`: a user is a paying customer (this will be useful in the future)
	- `admin`: can do **everything** inside the app
- `wallet_id` - this is the table that will track the money of a user

## Wallet
This table will hold the entire allocations of a **user** and will be updated on each **transactions**.
- `id`
- `user_id`
- `daily_profit` - the desired amount of profits that a user wants to make
- `exposure` - this will be updated at every `DEPOSIT` or `BUY` transaction and will be decreased at every `WITHDRAW` or `SELL`. 
- `liquid_funds` - will be increased by every `WITHDRAW` or `SELL`, and decreased at every `DEPOSIT` and `BUY`, **if** the user decides to use those funds. It cannot go lower than `0`  
- `profits` - will be increased at every 
- `project` - one-to-many relation with the projects
- `hodl` - one-to-many relation with the hodls positions

## Project
- `id`
- `name`
- `description`
- `exposure` - this field will track the amount that **user** has put into this project and will be calculated each time there is a new **transaction** of type `DEPOSIT` and decreased at each `WITHDRAW` until it reaches `0`
- `total` - is the sum of `DEPOSIT`s and `INTEREST`s (not yet withdrawn)
- `interests` - the money that are accumulating via **transactions** of type  `INTEREST` but that the user still has to `WITHDRAW`
- `withdraw` - is the sum of `WITHDRAW`s the user made
- `profits` - increase when `exposure` reaches `0` with each `WITHDRAW`
- `lock_period` - how long the project locks your funds
- `deposit_fee` - if there are fees on `DEPOSIT`
- `withdraw_fee` - if there are fees on `WITHDRAW`
- `increase_frequency` - this field will be an `enum` and will allow the **user** to set the frequency at which the project will increase the amount deposited
- `increase_amount` - this will allow to set a % at with the `current_holding` will be increased
- `compound` - if the increase needs to be compounded or not
- `user_id` - relation with the user that has created the project
- `transaction_id[]` - an array of investments that user has made (this will track deposit and withdraws) 

## Hodl
- `amount` - total amount of held tokens calculated at each `BUY` transaction
- `evaluation` - calculated with `amount` and `tokenPrice` each time it gets updated via `cron`
- `exposure` - increased at each `BUY` and decreased at each `SELL`, if it reaches `0` we start to increase `profits`
- `profits` - increase when `exposure` reaches `0` with each `SELL`

## Transactions 
- `id`
- `amount`
- `type` - it could be a **deposit**,  **withdraw**, **interest**, **buy** or **sell**
- `user_id`
- `project_id` - set if we're tracking a project
- `hodl_id` - set if we're tracking a token



## Token 
This table holds all the tokens I've been able to find.
- [ ] pull all tokens from 

# Future releases
- add the ability to add tags for project (this will be more useful when an admin creates central project)