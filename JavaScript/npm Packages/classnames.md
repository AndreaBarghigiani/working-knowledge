`classnames` is a great utility when you need to add/remove classes to an element based on conditions. 
> *Curiosity:* there is an alternative that declares to be more fast than this and exposes a similar API, if you want to have a look [here](https://www.npmjs.com/package/clsx). Its name is `clsx`.
Anyway most interesting thing of this kind of fns is that it helps us create complex structure for the class of our components with an easier API. Have a look at this:
```js
<div className={`hui-data-chart__tooltip hui-data-chart__tooltip--${ chartType }${
	'uptime' === chartType ||
	'seo-moz-rank' === chartType ||
	'uptime-response-times' === chartType ||
	'analytics' === chartType
		? ``
		: ` hui-data-chart__tooltip--show-tip`
		}>
```
This can get easily converted using `classnames` with:
```js
classnames( {
	[ `hui-data-chart__tooltip hui-data-chart__tooltip--${ chartType }` ]: true,
	'hui-data-chart__tooltip--show-tip': ! [
		'uptime',
		'seo-moz-rank',
		'uptime-response-times',
		'analytics',
	].includes( chartType ),
} );
```
The snippet present also a little transformation for checking the strings, I converted all the condition with a simple `includes()` array method.

### Use variable as class
In the snippet above I used the value of `chartType`, that is a string, to dynamically create the class name for the component.