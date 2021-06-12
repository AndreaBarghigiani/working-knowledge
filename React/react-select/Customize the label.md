In some cases you need to customize the label for your options, you can do it with styles and other times you need to customize the entire structure of the option.

## Pass custom data for the `options`
[As you can see from docs](https://react-select.com/styles) with the `styles` props your able to customize many of the components involved. For example if you want to style the hover color of the option you can do like so:
```js
const customStyle = {
	option: (styles) => ({
		...styles,
		":hover": {
			backgroundColor: '#f0f'
		}
	})
}
<Select styles={ customStyle } />
```
But with this implementation you will apply **the same color** for any options, what if you want to pass a custom color *inside the options array*?

As we should know all the options that we need to provide to this component live in the `options` prop with an array of object that is generally structured like so:
```js
const options = [
	{ value: 'val 1', label: 'Text displayed 1'},
	{ value: 'val 2', label: 'Text displayed 2'},
	{ value: 'val 3', label: 'Text displayed 3'},
];
```

One important note is that **we are not limited to pass object with that structure**, we're free to expand them as we wish:
```js
const options = [
	{ value: 'val 1', label: 'Text displayed 1', bgColor: '#000', color: '#fff'},
	{ value: 'val 2', label: 'Text displayed 2', bgColor: '#0f0', color: '#f0f'},
	{ value: 'val 3', label: 'Text displayed 3', bgColor: '#00f', color: '#ff0'},
]
```
Now each option in our select have additional info but **we're not using them**, yet 😉

### Use the custom data with `styles`
Now that we know that we can pass custom data in our `options` prop we can leverage them to customize each single option.

But how?

We can use the `data` prop passed from the state that's present in each option, here's how:
```js
const options = [
	{ value: 'val 1', label: 'Text displayed 1', bgColor: '#000', color: '#fff'},
	{ value: 'val 2', label: 'Text displayed 2', bgColor: '#0f0', color: '#f0f'},
	{ value: 'val 3', label: 'Text displayed 3', bgColor: '#00f', color: '#ff0'},
]

const customStyles = {
	option: (styles, {data}) => ({
		...styles,
		backgroundColor: data.bgColor,
		color: data.color
	})
}

<Select options={options} styles={ customStyles } />
```
Here I access the `data` to override the standard `background-color` and `color` CSS applied to each of them. 

This is quite cool but what about change the actual HTML that creates the option?

### Use custom HTML elements with `formatOptionLabel`
We can still use the custom properties we pass in our `options` array but this time we can leverage this to create entirely custom elements. 

Why this? 

For example you could pass the path to an image that you want to include in your label 😉

```js
const options = [
	{ value: 'val 1', label: 'Text displayed 1', path: 'your/custom/img1.png'},
	{ value: 'val 2', label: 'Text displayed 2', path: 'your/custom/img2.png'},
	{ value: 'val 3', label: 'Text displayed 3', path: 'your/custom/img3.png'},
]

formatOptionLabel = ( { label, path } ) => (
	<>
		{ path ? (
			<img
				src={ path }
				alt={ `${ label } Image` }
			/>
		) : null }

		<span>{ label }</span>
	</>
);

<Select
	options={ options }
	formatOptionLabel={ this.formatOptionLabel }
/>
```
The main difference with the previous approach is that `formatOptionLabel` **already gets the `data`** and in the example I simply deconstruct it to use only the value I care to customize the option.