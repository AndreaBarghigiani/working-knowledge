A really neat feature of databases is that you're able to group by a specific field your results.
I am writing this note while developing my Portfolio Tracker application so let's say that I have a table that keeps tracks of all the transactions that a user makes.

In the `Transaction` table I've separated those by the `type`, so they can be of type *INTEREST*, *DEPOSIT* or *WITHDRAW*.

`groupBy` is able to group the result **but** we need a way to tell it which kind of information we're looking for. In my case, I needed to get the sum of every field and this is what the query with Prisma looks like:
```js
ctx.prisma.transaction.groupBy({
	by: ["type"],
	_sum: {
		amount: true,
	},
});
```
This query will return the following:
```js
[
  {
    "_sum": {
      "amount": 200
    },
    "type": "DEPOSIT"
  },
  {
    "_sum": {
      "amount": 10
    },
    "type": "INTEREST"
  }
]
```

If I didn't come back yet to update this note, go and read the [doc page](https://www.prisma.io/docs/concepts/components/prisma-client/aggregation-grouping-summarizing#groupby-and-ordering)