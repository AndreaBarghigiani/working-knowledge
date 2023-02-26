Let's say that you get an [[array]] from a [[window.fetch()|fetch]] call, and you let the user select the items. To keep things simple, the `selection` array will only have the IDs of the item the user selected.

But now you need to let the user filter the selection, how do you do it while keeping all the data? `filter` is a perfect use case, but we are not comparing the items of the original array with just a simple value, we need to compare it with **each item** in `selection`.

We can solve it with a one-liner. You need to leverage `includes`:
```js
original.filter((item) => selection.includes(item.id))
```
This seems too good to be true, but it works!

Let's make a full example, shall we?

```js
const images = [
  {
    id: 'IOI3KCYsn0o',
    url: 'https://...',
    name: 'black Audi vehicle steering wheel'
  },
  {
    id: 'WPVtT0MEM00',
    url: 'https://...',
    name: 'aerial photography of cars on parking lot'
  },
  {
    id: '6lSBynPRaAQ',
    url: 'https://...',
    name: 'gray sports coupe parking during daytime'
  },
  {
    id: 'Hf1rAKkfMAg',
    url: 'https://...',
    name: 'white Audi A8 on the road surrounded of trees'
  },
  {
    id: '8qYE6LGHW-c',
    url: 'https://...',
    name: 'black mercedes benz coupe on gray asphalt road'
  },
  {
    id: '4gSavS9pe1s',
    url: 'https://...',
    name: 'white mercedes benz coupe on road during daytime'
  },
  {
    id: 'N9Pf2J656aQ',
    url: 'https://...',
    name: 'black Ford Mustang GT'
  }
]

const selection = [ 'IOI3KCYsn0o', '8qYE6LGHW-c', '6lSBynPRaAQ']
```