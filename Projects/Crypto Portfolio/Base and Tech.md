## Version 0.1

## Version 0.2

## Version 0.3

### Possible next features
* 

# Details
Base idea on how to structure the data scheme.

#### User:
* `email`
* `password`
* `role` *admin*,*user`

#### Token
* `name` 
* `network` 
* `cur_value` - This must be taken from API on a daily basis
* `desc` 
* `rebase` - If the token has rebase system
* `transaction_id` *(has_many)*

#### Transaction
* `token_id` *(belongs_to)*
* `amount` - the amount in the preferred currency of the user
* `token_amount` - convert the current amount into tokens
* `type` - this is an *enum* that defines the kind of transaction (*buy*, *sell*, *transaction*)