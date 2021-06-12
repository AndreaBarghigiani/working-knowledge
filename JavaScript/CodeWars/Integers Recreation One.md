> Kata: [Link](https://www.codewars.com/kata/55aa075506463dac6600010d/train/javascript)

For this one I needed to find some help on the web, not for code but to help me
out in separating the problem in smaller steps. Thanks goes to Florin Pop that has [written an article](https://www.florin-pop.com/blog/2019/06/jcc-integers-recreation-one/) based on this exact exercise.

Replication steps:
1. go over all numbers from `m` to `n`;
2. find all the *divisors* of the **current number** (`i`);
3. square all the *divisors* and add them up;
4. check if the resulting `sum` is a square number and if it is, store the number (`i`) and the `sum` into an array;

```js
function divisorsOf(num){
  let divisors = [];
  
  for (let i = 0; i <= num; i++){
    if (num % i === 0){
      divisors.push(i);
    }
  }
  
  return divisors;
}

function isSquare(x) {
    return Math.sqrt(x) % 1 === 0;
}

function listSquared(m , n){
  let res = [];
  
  for ( let i = m; i <= n; i++){
    // find all the divisors
    const divs = divisorsOf(i);
    
    const sumOfSquaredDivs = divs.reduce((acc, cur) => acc = acc + cur * cur, 0);
    
    if(isSquare(sumOfSquaredDivs)){
      res.push([i, sumOfSquaredDivs]);
    }
  }
  
  return res;
}
```
For this one I had to check step by step with Florin's code because this Math problems gives me hard times, not because I am not good at math (well I am not a genius either) but mostly because as a web developer I do not find many chances to practice its concept in a programming environment.

Well at the end of the day I am doing those Katas to push myself and learn something new so in case you need it here's are my finding for this:
* `Math.sqrt( num )` is a built in function that does most of the job for me and returns the square root of the `num` passed. Here we're using the `% 1` to see if it is an integer because if it is not it is not a square;
* this probably is the silliest and I got confused because I didn't study math in English but if you want to find the square of a `num` just multiply it by itself 😊

The other solutions where pretty similar than mine, in [this one](https://www.codewars.com/kata/reviews/55aa0d71c1eba8a65a000132/groups/5787db6eba5c4b1c8c0016ae) I find really interesting two things:
* the `reduce()` is called inline right after the `getDivisors()` invocation (I do this too usually but was too focused on math 😂)
* instead of `% 1 === 0` is used the more understandable function `Number.isInteger()` that wraps `Math.isSquare()`
* in the `for` loop of `getDivisors()` `num` gets divided by 2 and the check is `n % i`, need to check deeper this approach...
```js

function getDivisors (n) {
  var divisors = [];

  for (var i = 1; i <= n / 2; ++i) {
    if (n % i) {
      continue;
    }

    divisors.push(i);
  }

  return divisors.concat([n]);
}
```
A different solution that really got my attention has been the second one that solves the Kata with a single function with, tbh, a pretty easy to understand code:
```js
function listSquared(m, n) {
  var arr = [];
  for (var i = m; i <= n; i++){
    var temp = 0;
    for (var j = 1; j <= i; j++) {
      if ( i % j == 0) temp += j*j;  
    };
    if ( Math.sqrt(temp) % 1 == 0) arr.push([i, temp]);
  };
  return arr;
}
```
Well, math is everywhere in our world so better to get used to it 😂

Almost forgot but with this Kata I ranked up 🎉 Now I am a 6 kyu!