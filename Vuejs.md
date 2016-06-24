Vuejs Framework
===============

- Based on ECMAScript6 (ECMA Script is the standard, javascript an implementation of ECMA script)
- Find a quick Vuejs tour [here](https://vuejs.org/guide/).
- For CSS related lookups use [GetBootstrap](http://getbootstrap.com/).

### Debug Vuejs in Google chrome:

- Add "Vue.js devtools" plugin to Chrome
- open console, open vue plugin
- "send to console" ... gives access to all elements via $vm


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

- data() is actually a shortcut supported by ecma script 6. The compiler will transform this to a proper key-value pair:

        export default {
            data : function() {}
        }

- the same is true for every function within the `script` part

        data() {}
        # is the short form of
        data: function() {}

        methods: {
            calcSthg() {}
        }
        # is the short form of
        methods: {
            calcSthg: function() {}
        }

        computed: {
            someCompVal() {}
        }
        # is the short form of
        computed: {
            someCompVal: function() {}
        }
        # etc


### Binding variables to the `<template>`

- `<template>` contains html, data from the view model can be bound to the html using `{{}}`.
- the following example binds the script variable `message` to a specific `<div>` container within the template.

        <div>{{ message }}</div>

- bindings can be done to any part of the html e.g. also binding to html attributes rather than content.

        <div id="{{ someValue }}">

- bindings can also be java script single expressions and function calls

        {{ ok ? 'Yes' : 'No' }}

- bindings can only contain single expressions, no complete functions or statements

- expressions can be piped through filters e.g. built in `capitalize`

        {{ message | capitalize }}


### Directives

- directives are vue specific template attributes with a `v-` prefix.
- they are translated to bindings

        <a v-bind:href="someURL">follow me</a>
        # is the same as
        <a href="{{ someURL }}">follow me</a>
        # in both cases the attribute "href" is bound to the script variable "someURL"

- there are shorthands for two of the most commonly used directives, `v-bind` and `v-on`

        <a v-bind:href="url">follow me</a>
        # shorthand
        <a :href="url">follow me</a>

        <button v-on:click="doStuff">click me</button>
        # shorthand
        <button @click="doStuff">click me</button>


### Computed properties

- there should be virtually no logic in the `<template>`
- therefore expressions in bindings are restricted
- for more complicated computations the `<script>` part of vue provide computed properties.

        <template>
            <div> {{ a }} squared is {{ b }}</div>
        </template>

        <script>
            data() {
                return { a: 2 }
            },
            computed: {
                b: function() {
                    return this.a * this.a
                }
            }
        </script>

- NOTE: computed values are always bound to the state of their contained variables.
    - if the contained variables change, so will the computed values.


        <template>
            <div> {{ a }} squared is {{ b }}</div>
            <input v-model="a">
        </template>

        <script>
            data() {
                return { a: 2 }
            },
            computed: {
                b: function() {
                    return this.a * this.a
                }
            }
        </script>


### Dynamically handling css classes and styles

- use the `v-bind:class="someClassBinding"` shorthand `:class` syntax

- the following example will dynamically change the css class of a container

        <template>
            <div class="panel">
                <div :class="{ 'panel-header': isHeader, 'panel-body': isBody }">
                    Show me cool stuff!
                </div>
            </div>
        </template>

        <script>
            data() {
                return { isHeader: true, isBody: false }
            }
        </script>

- the following will outsource logic to the script part:

        <template>
            <div class="panel">
                <div :class="panelObject">
                    Show me cool stuff!
                </div>
            </div>
        </template>

        <script>
            data() {
                var panelObject = {
                    'panel-header': true,
                    'panel-body': false
                }
                return { panelObject }
            }
        </script>

### Dynamically show containers / branches

- use `v-if` or `v-show`

        <template>
            <div v-if="show-branch">
                <div>some text</div>
                <div v-show="show-div">some other text</div>
            </div>
        </template>

        <script>
            data() {
                return { show-branch: true, show-div: false }
            }
        </script>

- `v-if` is lazy - it is initialized with `false`; all its child components are only computed
    when it turns `true` the first time.
- components within `v-if` are always destroyed and newly computed when it changes its value.
- `v-show` is always compiled.
- prefer `v-if` when the child components contain dynamic, `v-show` when they contain static content.

### Handling arrays

- arrays are can be observables as well
- they can be included as e.g. dynamic lists using the `v-for` attribute

        <template>
            <ul v-for="item in listItems">
                <li>{{ item.mes }}</li>
            </ul>
        </template>

        <script>
            data() {
                var listItems = [{mes: 'one'}, {mes: 'two'}, {mes: 'three'}]
                return { listItems }
            }
        </script>

- the array is observed, changes to it will trigger View updates.

- be aware: if arrays are modified or replaced during runtime, they will usually not be destroyed. Vue.js uses
    heuristics to keep arrays for DOM element reuse e.g. if elements between old and new array overlap.

- NOTE: there are limitations to the observation of arrays. In the following cases changes are not updating the View:
    - when setting an item via index; e.g. `$vm.items[0] = {}`
    - when modifying the length of the array; e.g. `$vm.items.length = 0`


## Handling components

- every `<template>` is actually its own vue component that has to be registered to vue.
- every component has its own isolated scope.
- parent data should therefore not be directly used in a child components template.

### Using `props`

- data from a parent should always be passed to a child using the `props` field within a child component `data`

- NOTE: objects / arrays are passed by reference from parent to child via `props`. Changes to the content in the
    child also affect the parent!

## Handling parent - child

- a child can access its parent by `this.$parent`
- a parent has access to its children by the `this.$children` array
- don't do direct access
