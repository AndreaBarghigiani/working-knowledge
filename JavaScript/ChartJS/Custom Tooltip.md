A custom tooltip is defined passing a `custom` function in the options configuration like so:
```js
options = {
	type: 'multicolorLine',
	data: { ...	},
	options: {
		tooltips: {
			...
			enabled: false,
			custom: function ( tooltipModel ) { ... }
		}
	}
}
```

We **must** disable the on-canvas tooltip first in order to make the `custom` fn work.

`custom` fn gets the [[tooltipModel]]. 
