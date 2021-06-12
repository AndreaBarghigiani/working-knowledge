JavaScript gives us the [[substring()]] fn that return a new string that has been modified by its index. Today I was trying to replace a part of the string using a [[regex]] but I leveraged the [[indexOf()]] fn to find the index of the character that was at the beginnig of the string and this makes things easier since wheren't involved any complex regex.

```js
// Attempt with regex

// Looked to match strings like: '(Duplicate Oct 21 @ 7:59:38)', '(Duplicate Feb 1 @ 7:59:38)', '(Duplicate Ago 31 @ 12:9:8)'
const matchingString = /\(Duplicate (Gen|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dic) \d+ @ \d:\d+:\d+\)/;
```
Problem here is that time and dates can have different length also the [[replace()]] method is not working if the [[regex]] identify multiple instances.

```js
// Match returns an array in this case
string.match(matchingString)
```
For this reason I preferred search the index of `(` in order to remove the remaining string and change with the current one.
```js
const indexRemove = newReport.name.indexOf('(');
```
Once we got the index I can remove it:
```js
const newString = string.substring(0, indexRemove);
```