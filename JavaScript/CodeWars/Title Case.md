> Kata: [Link](https://www.codewars.com/kata/5202ef17a402dd033c000009/train/javascript)

Needed to implement a function that will capitalize all the words passed in it except the ones in `minorWords`.
```js
function titleCase(title, minorWords){
  function capitalize(word){
    return word.charAt(0).toUpperCase() + word.slice(1).toLowerCase(); 
  }
  
  const titleArr = title.split(' ');
  const minorWordsArr = 'undefined' !== typeof minorWords ? minorWords.toLowerCase().split(' ') : false;
  
  return titleArr.map( (word, i) => {
    if ( minorWordsArr && minorWordsArr.includes(word.toLowerCase())){
      return i === 0 ? capitalize(word) : word.toLowerCase();
    }
    return capitalize(word)
  }).join(' ')
}
```

With my solution I decided to create an utility fn to offload the capitalization of the matched work. A little gotcha has been the `toLowerCase()` call inside of the condition, this stopped me from pass the test at first attempt.

The following is the solution marked as *Best practice* and *Clever*, it took a similar path to mine except instead of make the `minorWords` a boolean it creates and empty array in case it was `undefined`, have mixed feelings about it because at the end we have a more complex condition IMO.

In addition there is no creation of `capitalize()` fn since the string is treated as an array that after the `toUpperCase()` is been joined back, this because the `title` has been transformed in `toLowerCase()` before spitting it and passing to `map`.

```js
function titleCase(title, minorWords) {
  var minorWords = typeof minorWords !== "undefined" ? minorWords.toLowerCase().split(' ') : [];
  return title.toLowerCase().split(' ').map(function(v, i) {
    if(v != "" && ( (minorWords.indexOf(v) === -1) || i == 0)) {
      v = v.split('');
      v[0] = v[0].toUpperCase();
      v = v.join('');
    }
    return v;
  }).join(' ');
}
```