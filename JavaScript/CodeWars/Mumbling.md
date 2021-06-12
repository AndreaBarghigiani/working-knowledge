> Kata: [codewars.com/r/8YQazg](https://codewars.com/r/8YQazg)

Asked to write a `accum` fn that will take a string and will transform it repeating each character based on its position, the first letter should be capitalized and have `-` as separator.
My solution was:
```js
function accum(s) {
  return s.split('').map( 
    (char,index) => {
      if (index === 0){
        return char.toUpperCase()
      }
      
      return char.toUpperCase() + char.repeat(index).toLowerCase();
    }
  ).join('-');
}
```
That at the end of the day is not that bad but have **one huge problem** that I discovered checking one of the most accepted answer.
```js
function accum(s) {
  return s.split('').map((c, i) => (c.toUpperCase() + c.toLowerCase().repeat(i))).join('-');
}
```
Beside the oneliner implementation did you spot my error?

Well I could **entirely avoid the `if` condition** because since we're repeating it based on `index` the first char has nothing to `repeat()` because index is 0 🤦‍♂️
