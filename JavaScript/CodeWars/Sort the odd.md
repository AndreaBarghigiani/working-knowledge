> Kata: [Link](https://www.codewars.com/kata/578aa45ee9fd15ff4600090d/train/javascript)

You have to create an algorithm that will swap only the odd numbers, this was the solution submitted by me:
```js
function sortArray(array){
  const odds = array
    .filter( item => item % 2)
    .sort((a,b) => a < b ? -1 : 1)
  return array
    .map( item => item % 2 ? odds.shift() : item)
}
```
With my approach I first [[filter]] and [[sort]] into a new array all the odd values, then I [[map]] into the original array and each time I find in it an odd value I take the first Item of my sorted array and put it in place of the 'original' odd number.

This is the same approach taken in the most accepted answer, kudos to me 🎉

Well **there is a minor difference** from mine... The sort callback fn that I made presents a minor difference, both are good but mine is just longer:
```js
// my sort callback
(a, b) => a < b ? -1 : 1;
// accepted solution
(a, b) => a - b;
```
Basically I am taking the longer path because as you'll find in [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) the accepted one is basically the sorting logic for numbers, give it a read to the comparing fns 😉