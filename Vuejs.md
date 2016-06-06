Vuejs Framework
===============

- Based on ECMAScript6 (ECMA Script is the standard, javascript an implementation of ECMA script)
- Find a quick tour [here](https://vuejs.org/guide/).

## Getting started

- Requires npm v >= 3.9
- If lesser version is installed, remove from system. Check, that ```which npm``` does not yield any result.
if there still is a result e.g. in /usr/local/lib or /usr/local/bin, unlink these before continuing.
- DL latest version of [nodejs from here](https://nodejs.org/). Extract to program folder of choice.
- Create version independent link; add version independent link to $HOME/.profile
(don't forget to ```source``` afterwards), e.g.:

        if [ -d "$HOME/work/software/nodejs" ] ; then
            PATH="$PATH:$HOME/work/software/nodejs/bin"
        fi

- clone vue.js project
- Install npm dependencies in main folder ```npm install```. This will add all dependencies
specified in "package.json" to the node_modules folder.
- Start local application using ```npm run dev```
