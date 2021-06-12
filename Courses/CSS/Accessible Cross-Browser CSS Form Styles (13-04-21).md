> Notes from the [Accessible Cross-Browser CSS Form Styles](https://egghead.io/courses/accessible-cross-browser-css-form-styling-7297) course on Egghead

## Text fields
A text input is the default that we can use, browser are so used to it that we can omit the `type` attribute. However is still a best practice to use it:
```html
<input type="text" />
```
Generally each `input` is used with a `label` and to also improve accessibility we can add a `for` attribute to `label` that will contain the `id` set for the connected `input`:
```html
<label for="text-input">Text input</label>
<input type="text" id="text-input" />
```
`input` also holds the `name` attribute that is used to find it when form it's submitted. 