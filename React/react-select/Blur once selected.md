In order to remove the `focus` from the input once the user has selected the desired option we can leverage the `blur()` method, little secret you have to invoke it from the current `ref` that was already applied to the `Select`.

### Class syntax component
```js
class CustomComponent extends React.Component {
	constructor(props){
		// super and state...
		this.customRef = React.createRef();
	}

	render(){
		return (
			<Select 
				ref={ this.customRef }
				onChange={ () => {
					// Do your stuff
					this.customRef.current.blur()
				}}
				// other props...
			/>
		)
	}
}
```
### Hook syntax component
TBD