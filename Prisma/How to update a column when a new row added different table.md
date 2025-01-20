This is a standard JavaScript procedure in Prisma. Let's say that I have an app where I keep track of the money I am saving, we could have a `wallet` table with a `total` column that keep tracks of my entire savings.

To make things organized I want to track each time I add something, so I've created a `transaction` table with the `amount` field that will let me keep track of how much I add to my `wallet` each time.

So I have to pop in while I create the same transaction, this is the code I came up with:
```js
await prisma.transaction.create({ data: input });
await prisma.wallet.update({
  where: {
    id: input.walletId,
  },
  data: {
    total: {
      increment: input.amount,
    },
  },
```
As you can see, first I add the new `transaction` to the table with the `create` method and the values that comes from `input`, after that I jump into the `wallet` table and I select the row via the `id` and `increment` the `total` with the `amount` that the user has submitted.

### How to use `increment` or `decrement` conditionally
Adding is fun, but I would also like to be able to remove from `wallet` in case I need to track when I took something for emergencies.

Since I am still adding a new `transaction` with a different `type` I need to decide on 'what to do' calculating the `total` in my wallet.

This is what I come up with but probably there's still a more elegant solution.

```js
await prisma.transaction.create({ data: input });
await prisma.project.update({
  where: {
    id: input.projectId,
  },
  data: {
    currentHolding: {
      ...(input.type === "withdraw"
        ? { decrement: input.amount }
        : { increment: input.amount }),
    },
  },
});
```
This time I leverage the power of the [[spread operator | spread]] to use the value returned from the [[ternary operators | ternary]]