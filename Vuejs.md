Vuejs Framework
===============

- Based on ECMAScript6 (ECMA Script is the standard, javascript an implementation of ECMA script)
- Find a quick Vuejs tour [here](https://vuejs.org/guide/).
- For CSS related lookups use [GetBootstrap](http://getbootstrap.com/).

## Getting started

- Requires npm v >= 3.9
- If lesser version is installed, remove from system. Check, that `which npm` does not yield any result.
if there still is a result e.g. in `/usr/local/lib or /usr/local/bin`, unlink these before continuing.
- DL latest version of [nodejs from here](https://nodejs.org/). Extract to program folder of choice.
- Create version independent link; add version independent link to `$HOME/.profile`.
(don't forget to `source` afterwards), e.g.:

        if [ -d "$HOME/work/software/nodejs" ] ; then
            PATH="$PATH:$HOME/work/software/nodejs/bin"
        fi

- clone vue.js project
- Install npm dependencies in main folder `npm install`. This will add all dependencies
specified in "package.json" to the node_modules folder.
- Start local application using `npm run dev`

## Structure of a vue file
- `<style>` block: additional styles specific for the current page.
- `<template>` block: contains the structure of a page using HTML elements; these can contain css attributes
as well as vue specific attribute to dynamically include data. Elements can be bound to different views using v-bind.
- `<script>` block: contains imports of further views and bindings to data. only parts within `export` or
`export default` can be accessed outside of the file.


- export blocks can contain functions like e.g. `data()`, `methods`, `events`, `props`, `mixins`
- `props` can be used to share bound data between various views (using `twoway: true`)

## Project structure

#### main folder: package.json
- Contains all required packages and dependencies

#### main folder: webpack.config.js
- defines which modules to load and with which settings and how and where the actually created javascript file
will be put. can specify different settings when creating the output javascript files. e.g. settings for png limit
can specify up to which file size images should be encoded with base64 rather than providing a link to an image.

#### src.App.vue
- Main entry point to the App.


## Random notes

### `<template>`
- `<template>` should always contain only a single child tag. This child tag can of course contain multiple tags.

### export default
- stuff that is accessible to the outside of the current file / component
- has to return at least an empty data object.

        export default {
            data() {}
        }

- this thing is at the moment and object w/o a name. It will be given a name, if it is imported somewhere else:

        import NewName from "./someFile.vue"

- data() is actually a short cut supported by ecma script 6. The compiler will transform this to a proper key-value pair:

        export default {
            data : function() {}
        }
