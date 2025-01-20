# Withdraws
We consider **withdraws** as money that the user **takes from interests**. 
We need to check how each withdrawal will affect `exposure` and `profits`. 

Each withdraws will update the following tables:
- `project`: updating `profit`, `interest` columns
- `transaction`: creating a new `WITHDRAW` row
- `wallet`: updating the `liquid_fund` value
## Withdraw > Exposure (50 > 25)
In this case we have a greater withdrawal we need to:
- set `exposure` to `0`
- increase `profits` by **the difference** (withdraw - exposure)
- decrease `interest` by the `withdraw` amount
- add `withdraw` amount to `liquid_fund`
#### Example
![[CleanShot 2023-06-28 at 05.34.41.jpg]]
## Withdraw == Exposure (25 == 25)
- set `exposure` to `0`
- decrease `interest` by the `withdraw` amount
- add `withdraw` amount to `liquid_fund` in `wallet` table
## Withdraw < Exposure (15 < 25)
- decrease `exposure` by `withdraw` amount
- decrease `interest` by `withdraw` amount
- add `withdraw` amount to `liquid_fund` in `wallet` table
#### Example 
![[CleanShot 2023-06-28 at 05.31.40.jpg]]