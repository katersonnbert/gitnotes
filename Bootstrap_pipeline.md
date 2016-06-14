Build bootstrap project with custom less files
==============================================

## Install dependencies
To build a custom bootstrap project, grunt is used. grunt requires nodejs and jekyll.

- Get nodejs and npm (required for bootstrap and jekyll)

        sudo apt-get install nodejs nodejs-dev npm node-less

- Get ruby(required for jekyll)

        sudo apt-get install ruby ruby-dev make gcc

- Get jekyll and rouge (both required for grunt)

        sudo gem install jekyll
        sudo gem install rouge

- Get grunt (required for bootstrap)

        sudo npm install -g grunt-cli


## DL bootstrap distribution
Download bootstrap source code distribution of choice and extract to directory of choice.

## Prepare custom less files
Prepare all custom less files that are supposed to replace original bootstrap definitions.
Modify bootstrap.less file to include all custom files.


# Build custom bootstrap css
## prepare files
copy modified bootstrap.less file and all additional custom files to the `/bootstrap/less/` directory,
replacing the original `bootstrap.less` file.

## build bootstrap using grunt
- prepare bootstrap directory for the usage with grunt: move to main `/bootstrap/` folder, there run

        sudo npm install

- build css from modified less files

        grunt dist

- new css files have been built in folder `dist/`. Copy `bootstrap.css` or `bootstrap.min.css` to your
project stylesheet folder.



# Compile custom bootstrap.css file using lessc
- Get nodejs and npm

        sudo apt-get install nodejs nodejs-dev npm node-less

- download bootstrap source code distribution of choice and extract to directory of choice
- clone the gnstyle project to a directory of choice

        git clone git@github.com:G-Node/gnstyle.git

- modify the bootstrap path variable in `_G-Node-bootstrap.less`:

        @bootPath ... path to where the original bootstrap less files are located

- compile the custom `boostrap.css` file using less


# Using custom bootstrap git in a play framework:
- move to `app/assets/stylesheets` folder
- clone the gnstyle project, follow the cloning procedure exactly:

        git clone git@github.com:G-Node/gnstyle.git my-subproject
        git add my-subproject/

- only then the inner git will be set up properly to play nice with the outer git
- now you can properly commit changes from within both git repositories

## Build bootstrap from custom less in play framework
- change content of the path variable in "G-Node-bootstrap.play.less" to where the original
bootstrap less files can be found.
- make sure the href in "template.scala" references the correct path to *.min.css files that are
created when the project is compiled.
