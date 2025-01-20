# `npm list`
`npm list` is an utility that lists all the packages inside a project and `npm ls` is an alias.

if you don't see the depth, meaning you only see the first level, you can specify how deep you would like to go with `--depth=<num>`.

You can also check the list of packages that will be shipped to production because they live inside `dependencies` with `--prod` (or `--production`.

Or the ones that live inside `devDependencies` with `--dev` (or `--development`).

But you can also list the ones that live in global with `--global`