Vuejs Framework
===============

- Based on ECMAScript6 (ECMA Script is the standard, javascript an implementation of ECMA script)
- Find a quick Vuejs tour [here](https://vuejs.org/guide/).
- For CSS related lookups use [GetBootstrap](http://getbootstrap.com/).

### Debug Vuejs in Google chrome:
- Add "Vue.js devtools" plugin to Chrome
- open console, open vue plugin
- "send to console" ... gives access to all elements via $vm


## Good to knows
- Vue does not support IE8 and below, because it requires ECMAScript 5.
- Vue compiles templates by recursively walking the DOM tree.


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

- NOTE: computed properties are cached. In case a computed property contains a function call, that
    should be re-evaluated every time the property is accessed, turn caching off.

        <script>
            data() {
                msg: "hurra!"
            },
            computed: {
                example: {
                    cahe: false,
                    get: function() {
                        return Date.now() + this.msg
                    }
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
- parents are usually not aware of a child components state. Therefore vue-directives (`v-`)
    dealing with the child will not work in the parent component.

        # will not work
        <child-component v-show="childProperty"></child-component>

## Content distribution using `<slot>`

### Slots when handling parent / child content

- Parent content will be discarded in child elements, unless a child component contains at least one `<slot>` tag.
- The content of `<slot>` tags is a "fallback content", that will only be displayed,
    if there is no other content from the parent.

        # parent template 1
        <child-component1>
            <div>This is my parent content!</div>
        </child-component1>

        # parent template 2
        <child-component2></child-component2>

        # child template for both components 1 / 2
        <div>
            <h1>child header</h1>
            <slot>
                <div>I shall be displayed as fallback!</div>
            </slot>
        </div>

        # The output of child component 1:
        <h1>Child header</h1>
        <div>This is my parent content</div>

        # The output of child component 2:
        <h1>Child header</h1>
        <div>I shall be displayed as fallback!</div>

### Named slots

- named slots can be used to define how multiple parent content is distributed in child elements.

        # parent template
        <child-content>
            <p slot="one">Text one</p>
            <p slot="two">Text two</p>
            <p>Default text</p>
        </child-content>

        # child-content template
        <div>
            <slot name="one"></slot>
            <slot></slot>
            <slot name="two"></slot>
        </div>

        # The ouutput will be:
        <div>
            <p slot="one">Text one</p>
            <p>Default text</p>
            <p slot="two">Text two</p>
        </div>


## Dynamic components

- whole components can be switched dynamically using the `<component>` tag.
- the state of components that have been switched can be cached and kept in the background
    using the `keep-alive` attribute.
- the `activate` hook can be used to perform initial setup of a switched in component before it will be displayed.
- the `transition-mode` parameter attribute can be used to define how two dynamic components should behave when
    they are switched. e.g. using a fade effect during the switch.


# Some Vuejs internals

## How changes are tracked

- when a JavaScript object is passed to a Vue instance as a `data` option, Vuejs will convert
    all the objects properties to getters and setters using ES5 `Object.defineProperty()`.
- these properties are used by Vuejs to send notifications if their content is accessed or changes.
    NOTE: browser consoles display getter/setter differently when they are logged!
- in detail, vor every directive or data binding in a vue template, a "watcher" object is created, that
    records any properties "touched". In these cases the watcher object triggers corresponding DOM updates.
    [Check this for details](https://vuejs.org/images/data.png).

- NOTE: getters and setters are created during instance initialization. Properties that are added to `data` after
    this initialization are not tracked, since the getters and setters will not be created after
    the instance is initialized! Always prefer to declare reactive properties in the data option!


## Custom Directives

- register custom directives globally by using `Vue.directive(idString, function definition)`.

        Vue.directive('cstm', { // define stuff })

        # use in a template with the "v-" prefix
        <div v-cstm="hurra"></div>

- register custom directives locally by including them in a components `directives` option.
- directives can be registered as attributes (`<div some-directive="">`) or directly as elements (`<some-directive>`).

        Vue.elementDirective('cstm-el', { //define stuff })

        # use as
        <cstm-el></cstm-el>

- NOTE: if a custom directive is used on an object and it should track changes within this
    object, the property `deep: true` has to be used

        Vue.directive('custom', {
            deep: true,
            update(obj) { // do stuff }
        })


### Hook functions

- Custom directives can be created using some of the following hooks:
    - `bind`    ... called when the directive is first bound to the element.
    - `update`  ... called after `bind` and every time the binding value changes.
    - `unbind`  ... called once when the directive is unbound from the element.

            # register global directive
            Vue.directive('cstm', {
                bind: function() {},
                update: function() {},
                unbind: function {}
            })

            # use as
            <div v-cstm="someValue"></div>

### Custom attributes with custom directives

- when defining custom directives, an array of custom attributes can be defined via option `params`.

        Vue.directive('cstm', {
            params: ['attr-1', 'attr-2'],
            bind() {
                console.log(this.params.attr-1)
            }
        })

        # use as
        <div v-cstm attr-1="hurra" attr-2="die gams"></div>

### Bidirectional custom directives

- when a custom directive is supposed to write information back to the Vue instance, use option `twoWay: true`

        Vue.directive('cstm', {
            twoWay: true,
            bind() {
                this.setHandler = function() {
                    # this.el ... element the directive is bound to
                    this.set(this.el.value)
                }.bind(this)
                this.el.addEventListener('input', this.setHandler)
            },
            unbind() {
                this.el.removeEventListener('input', this.setHandler)
            }
        })

### Terminal directives

- Vue walks the DOM tree recursively when compiling templates.
- It stops if it reaches a "terminal" directive like `v-if` or `v-for`.
- It is possible to write custom terminal directives by adding the `terminal: true` option.


## Mixins

- Mixins can be used to write reusable functionalities for Vue components.

- A mixin can contain any component option like `methods` or `computed`.

        var custMixin = { created: function() { console.log('mixin hook') } }

        new Vue({
            mixins: [custMixin],
            created: function() { console.log('component hook') }
        })

        # will output
        # "mixin hook"
        # "component hook"

- "Overlapping" options  will be automatically "merged" by different strategies:
    - hook functions with the same name are merged into an array, all of them will be called.
    - mixin hooks will always be called before component hooks.
    - options handling objects e.g. `methods` or `components` will be merged into the same object.
    - here component options take priority over mixin options.

- There is the option to create custom merge strategies using `Vue.config.optionMergeStrategies`.

- There is the option to create global mixins `Vue.mixin({ // do stuff })` but use this option carefully...

