> Kata: [Link](https://www.codewars.com/kata/54da539698b8a2ad76000228/train/javascript)

Take an array of strings that represent directions and see if the path that those directions express will make a path that will bring you back at the starting point in 10min (10 directions).

Since we must receive an array of exactly 10 items the first thing I do is return `false` in case it is not like so, we save time as well.

Then I created a simple `path` array where the item at index `0` are considered vertical movements and the ones at index `1` are horizontal ones. Checking with a simple `switch` case lets me increase/decrease the values for specific movement and I'll return `true` **only if** both values are 0 at the end of the operation.
```js
function isValidWalk(arr){
  if(arr.length !== 10) return false;
  const path = [0, 0];
  arr.forEach( direction => {
    switch(direction){
      case 'n':
        path[0]++;
        break;
      case 's':
        path[0]--;
        break;
      case 'e':
        path[1]++;
        break;
      case 'w':
        path[1]--;
        break
      default:
        return false;
    }
  });
  return path[0] === 0 && path[1] === 0;
}
```
Probably I could improve readability but it works 😊

From the users solutions the most accepted one is this:
```js
function isValidWalk(walk) {
  var dx = 0
  var dy = 0
  var dt = walk.length
  
  for (var i = 0; i < walk.length; i++) {
    switch (walk[i]) {
      case 'n': dy--; break
      case 's': dy++; break
      case 'w': dx--; break
      case 'e': dx++; break
    }
  }
  
  return dt === 10 && dx === 0 && dy === 0
}
```
As you can see in this solution the user created two separate variables to keep track of the distance, this is the first difference from my approach and is surely more readable. Anyway even if it is cleaner I do not like the fact that it waits until the end to check if the walk is exactly 10 steps. 
We are working with a simple array of 10 items but you never have to trust the user 😉

The most clever solution that community has voted is the following and tbh it is really clever!
```js
function isValidWalk(walk) {
  function count(val) {
    return walk.filter(function(a){return a==val;}).length;
  }
  return walk.length==10 && count('n')==count('s') && count('w')==count('e');
}
```
It's creator made the utility fn that simply count the numbers of time a specific direction is added. If the directions are the same for both axis than it means that we send our user back to first square.

In this case checking for the `length` before the `return` statement **makes a lot of sense** because we will run the `count()` fn only if the array has the exact number of items.