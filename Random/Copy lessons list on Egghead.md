Simple console scripting does the trick. 

```js
const nodes = document.querySelectorAll('.simplebar-content li .leading-tight')
const text = Array.from(nodes).map(item => item.textContent)
```

As long as Egghead keeps the same structure you can copy/paste the result array.