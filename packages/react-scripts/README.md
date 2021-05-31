# react-scripts with custom tsconfigs

React scripts with support for custom build and start (watch) tsconfigs like this:

`"build": "env-cmd carlribbegaardh-react-scripts build"`

```
TSCONFIG_PROD=tsconfig.prod.json
TSCONFIG_DEV=tsconfig.dev.json
```

# react-scripts

This package includes scripts and configuration used by [Create React App](https://github.com/facebook/create-react-app).<br>
Please refer to its documentation:

- [Getting Started](https://facebook.github.io/create-react-app/docs/getting-started) – How to create a new app.
- [User Guide](https://facebook.github.io/create-react-app/) – How to develop apps bootstrapped with Create React App.

## background

If you are working in a monorepo with vscode you likely have a tsconfig like this:
```
{
  "extends": "../../tsconfig-base.json",
  ...
}
```
And a base tsconfig like this:
```
{
  "compilerOptions": {
    "baseUrl": "./packages",
    "paths": {
      "@your-project/*": ["*/src"],
       ...
  ...
}
```
or something similar.

While this is nice, providing navigation between projects, and an updated linting
whenever you edit the library projects, it has a negative side when it comes to building.
A setup like this causes the compiler to consider the source files instead of the build or dist folders
of the library projects in your setup.
Ideally you want a build tsconfig free from paths into src.

The most often used solution is to update the config on the fly while building, but with this package
you can configure separate tsconfigs for build and watch, making the build process much easier and faster.

## special instructions

This package is only available while this PR is open.
https://github.com/facebook/create-react-app/pull/11031

The react-scripts bin file is named carlribbegaardh-react-scripts in this package
to be able to coexist along the normal react-scripts in a monorepo.

### Usage

`yarn add -D carlribbegaardh-react-scripts`\
`yarn workspace {your-workspace} add -D carlribbegaardh-react-scripts`

Then change your package.json build (and/or start) command from\
`"build": "react-scripts build"`\
 to\
 `"build": "carlribbegaardh-react-scripts build"`

## custom tsconfig!

1. Add `env-cmd` to your project
2. Copy tsconfig.json to tsconfig.prod.json and tsconfig.dev.json
3. Adjust them to build vs start your project the optimal way.
4. Add a .env file like this:

```
TSCONFIG_PROD=tsconfig.prod.json
TSCONFIG_DEV=tsconfig.dev.json
```

5. Change your tsconfig.json to\
`"build": "env-cmd carlribbegaardh-react-scripts build"`\
`"start": "env-cmd carlribbegaardh-react-scripts start"`

6. Change your react-app-env.d.ts to\
`/// <reference types="carlribbegaardh-react-scripts" />`

Now the environment will behave like this:\
**tsconfig.json** is the file your editor will pick up.\
**tsconfig.prod.json** is the file used during build your cra app.\
**tsconfig.dev.json** is the file used when you start your cra app.