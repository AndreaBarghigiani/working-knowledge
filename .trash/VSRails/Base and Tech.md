Create a VS Code Remote Container able to jump start your work in Rails.

## Version 0.1
- 
### Possible features
- **customizations -** give ability to change specs and gems in the container
- **livereloading -**   implement `hotwire-livereload` ([gem](https://github.com/kirillplatonov/hotwire-livereload), [article](https://kirillplatonov.com/posts/hotwire-livereload/))
- **autocompletition, error highlight, go to definition... -** implement `sorbet` both as a [gem](https://sorbet.org/docs/adopting) and [VS extention](https://sorbet.org/blog/2022/01/06/open-sourcing-sorbet-vscode) ([docs]https://sorbet.org/docs/vscode)
- **lint/format -** implement `rubocop` soth as [gem](https://rubocop.org/) and [VS extention](https://marketplace.visualstudio.com/items?itemName=misogi.ruby-rubocop)
- **TailwindCSS integration -** Tailwind will be supported out of the box with the [official ext](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss) and the [opinionated way to organize classes](https://marketplace.visualstudio.com/items?itemName=heybourn.headwind) (remember to implement the correct settings for VS Code)
- **Stylelint** [ext](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)
- **Editorconfig** [ext](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)
- **ES Lint** [ext](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- **YAML** [ext](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)
##### Maybe not needed
- **autoclose `end` -** cheking out if [endwise](https://marketplace.visualstudio.com/items?itemName=kaiwood.endwise) is still needed with `sorbet` and `rubocop` are working
- **snippets -** probablu the [extention Ruby](https://marketplace.visualstudio.com/items?itemName=hridoy.rails-snippets) on Rails it's not needed
-  **Intellisense** [ext](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode)

## Gem list
Many features require some gems to work properly, here's a small collection of them:
- `hotwire-livereload`
- `sorbet`
	- `sorbet-rails`
- `rubocop`
	- `rubocop-sorbet`

## Custom settings for VS Code
**TailwindCSS**

```json
"tailwindCSS.includeLanguages": {
  "*.erb": "html",
  "*.html.erb": "erb"
},
"tailwindCSS.experimental.classRegex": ["class:\\s*\"([^\"]*)\""],
"files.associations": {
  "*.html.erb": "erb",
  "*.erb": "erb"
},
```