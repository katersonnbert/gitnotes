 Outline how to setup a nice Ubuntu Linux desktop
===================================================

- IMPORTANT NOTE: before new install do a thorough backup! especially config files & ssh keys!

- NOTE: Good to know under debian/ubuntu: when using apt-get make sure the local package index is up to date:

        sudo apt-get update

#### NOTE: First of all good to know under Linux:
- `$HOME` ... contains absolute path to the home folder
- `$PATH` ... contains all directories which are included when looking for an executable


#### Adding custom executable path under Linux:

- Set up a bin folder that is included in the Linux $PATH which contains all the links to starting programs.
- Check out this [link](http://www.troubleshooters.com/linux/prepostpath.htm) for further information.
- Folders can be easily added to the beginning and the the end of the variable - depending on where the
application is first found, this will be executed.

Example:
- In folder ~ (absolute `/home/[username]` in our example)

        mkdir bin

- Check if path has been automatically added

        cat $PATH

- If not, then execute the following to recompile the `.profile` file

        source .profile

- Check path again

        cat $PATH

- If its still not in there, then add it manually to the beginning of the PATH:

        export PATH=/home/[user]/bin:$PATH

##### Symbolic links
- Create symbolic links to executables in custom bin folder.
- As example add startup shell script of application activator
can be used to easily switch between different distributions of the same application

        cd bin
        ln -s /home/[user]/work/software/activator-1.2.12/activator activator


##### Adding an application to the quick launcher bar:
- create file `[name of application].desktop` somewhere, open it with an editor, add at least the following:

        [Desktop Entry]
        Type=Application
        Name=[Name of application]
        Comment=[Comment]
        Icon=[path to icon]
        Exec=[path to shell script]
        Terminal=false
        Categories=[Linux application categories e.g. Development;IDE;Java;]

- DO NOT CREATE DIRECTLY IN THE HIDDEN FOLDER
- DO NOT USE QUOTES WHEN SETTING THE ICON PATH
- move the created .desktop file to hidden folder `~/.local/share/applications`
- manually edit the properties (`properties -> permissions -> execute`) to make it executable
- draw it onto the quick launcher bar.

##### Edit launcher links in Ubuntu 14
- Edit an existing quick launcher link: right click, properties, change whatever you like.
- Remove an existing quick launcher link: alt + right-click


## Before a migration:
- check directories, copy all interesting stuff
- export bookmarks
- copy source code not managed by git
- save ssh keys and congig/profile files


## Setting up new Linux Ubuntu computer
- Install latest Ubuntu LTS
- Here are a couple of installation notes for Ubuntu 16 LTS
    - [Ubuntu from USB](https://help.ubuntu.com/community/Installation/FromUSBStick)
    - [Ubuntu 16 installation guide](http://www.tecmint.com/ubuntu-16-04-installation-guide/)
    - [Dual boot alongside Windows](http://www.tecmint.com/install-ubuntu-16-04-alongside-with-windows-10-or-8-in-dual-boot/#)
    - [Customize Ubuntu 16](http://www.tecmint.com/things-you-mostly-need-to-do-after-installing-ubuntu-16-04/4/)

- Update Ubuntu (software center)
- create folders

        bin
        chaos
        chaos/dl
        chaos/DC
        chaos/DNC
        chaos/work
        chaos/software


### Install latest Oracle Java

- find infos:
    - [Java via ppa](http://tecadmin.net/install-oracle-java-8-jdk-8-ubuntu-via-ppa/)
    - [Running Intellij on Ubuntu](http://wiki.jetbrains.net/intellij/Installing_and_running_IntelliJ_IDEA_on_Ubuntu)


        sudo add-apt-repository ppa:webupd8team/java
        sudo apt-get update
        sudo apt-get install oracle-java8-installer

- check java version

        java -version

- set up java environment

        sudo apt-get install oracle-java8-set-default


### Firefox - update ff preferences:
- privacy
- select individual history
- set third party cookies to never
- set ask every time

#### Firefox addons:
- Adblock plus
- Youtube video and audio downloader
- Print pages to Pdf


### Install Chromium

    sudo apt-get install chromium-browser

- open chromium from terminal, lock to launcher


### Install HDFView

    sudo apt-get install hdfview

### Install nixio dependencies:

    sudo apt-get install libhdf5-serial-dev libcppunit-dev cmake build-essential
    #sudo apt-get install libhdf5-7 #does not work

    sudo apt-get install libboost-all-dev
    sudo apt-get install libyaml-cpp-dev
    sudo apt-get install cython cython3



## Setup ssh-keys:
Copy folder `~/.ssh`, makes it easier plus keeps old keys, otherwise:

- create `.ssh` folder in home folder folder

        cd
        mkdir .ssh

- set folder properties

        chmod go-rwx -R .ssh

- create new general key

        cd .ssh
        ssh-keygen
        # say yes to everything, no pw

- create new github key

        ssh-keygen

- call key "id_rsa_github"


### Install curl
    sudo apt-get install curl

### Install Maven
    sudo apt-get install maven

### Install and setup Git
- Dl and install [git](http://git-scm.com/)

        sudo apt-get install git

- check if `~/.gitconfig` exists, create file otherwise
- copy the following to file `~/.gitconfig`

        [user]
            name = M. Sonntag
            email = michael.p.sonntag@gmail.com
        [color]
            ui = auto
            branch = auto
            diff = auto
            interactive = auto
            status = auto
        [branch "master"]
            remote = origin
            merge = refs/heads/master
        [core]
            editor = gedit
            autocrlf = input
            whitespace = trailing-space
        [alias]
            st = status
            ci = commit
            co = checkout
            br = branch
            me = merge
        [push]
            default = matching

- open `~/.bashrc` (ctrl-h in file browser to show hidden files)
- add the following to the end of the file

        # Git state
        function parse_git_dirty {
        [[ $(git status 2> /dev/null | tail -n1) != "nothing to commit (working directory clean)" ]] && echo "*"
        }
        function parse_git_branch {
        git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e "s/* \(.*\)/[\1$(parse_git_dirty)]/"
        }
        export PS1='\u@\h \[\033[1;33m\]\w\[\033[0m\]$(parse_git_branch)$ '

- if required: setup github with new ssh key.

        cat id_rsa_github.pub

- prints public key, copy everything
- log in to github
    - settings
    - ssh keys
    - new SSH key
    - paste public key

### Install tree commandline programm (prints file structure tree from the current directory)
    sudo apt-get install tree


### Install Gedit (if its not there)
        sudo apt-get install gedit

- change gedit settings
    - Edit -> Preferences -> View ... Display line numbers
    - Edit -> Preferences -> Font & Colors -> Oblivion
    - Edit -> Preferences -> Editor ... Tab width 4, Insert spaces instead of tabs, autosave files every 5min


### Install terminator terminal
        sudo apt-get install terminator

- Setup terminator config file

        cd ~/.config/
        mkdir terminator
        cd terminator
        gedit config

        add the following:
        [global_config]
          title_transmit_bg_color = "#562946"
          title_inactive_bg_color = "#2f0922"
        [keybindings]
        [profiles]
          [[default]]
            use_system_font = False
            borderless = true
            font = Monospace 12
            background_color = "#2f0922"
            foreground_color = "#ffffff"
        [layouts]
          [[default]]
            [[[child1]]]
              type = Terminal
              parent = window0
            [[[window0]]]
              type = Window
              parent = ""
        [plugins]
- open terminator from terminal, lock to launcher

# Set up python

## Install python and python libraries

### Install matplotlib dependencies for later
    sudo apt-get install libfreetype6-dev libpng-dev

### Install python-odml dependencies for later
    sudo apt-get install libxml2-dev libxslt-dev

### Install python package manager and virtualenv
    sudo apt-get install python-pip python-pip3
    sudo pip install --upgrade pip
    sudo pip install virtualenv

### Prepare folders for python virtualenv
    cd /home/Chaos/software
    mkdir pyvirtualenv
    cd pyvirtualenv

### Create and setup python2 virtualenv
    virtualenv --system-site-packages pymain

- activate py2 virtualenv

        source /home/msonntag/Chaos/software/pyvirtualenv/pymain/bin/activate

- install python libraries

        pip install numpy
        pip install sphinx
        pip install matplotlib
        pip install h5py
        pip install scipy
        pip install pyyaml
        pip install quantities
        pip install coveralls
        pip install cpp-coveralls
        pip install mock==1.0.1
        pip install rdflib
        pip install alabaster
        pip install future
        pip install python-magic

- install neo dependencies

        pip install numexpr
        pip install tables

- install odml dependencies

        pip install enum
        pip install lxml

- install jupyter + jupyter

        pip install jupyter

- end virtualenv

        deactivate


### Create and setup python3 virtualenv
    virtualenv --system-site-packages -p /usr/bin/python3 py3main

- install python libraries

        pip3 install numpy
        pip3 install sphinx
        pip3 install matplotlib
        pip3 install h5py
        pip3 install scipy
        pip3 install pyyaml
        pip3 install quantities
        pip3 install coveralls
        pip3 install cpp-coveralls
        pip3 install mock
        pip3 install rdflib
        pip3 install alabaster
        pip3 install python magic

- install neo dependencies

        pip3 install numexpr
        pip3 install tables

- install odml dependencies

        pip3 install lxml
        # python3 does not need to install enum

- install jupyter + ipython

        pip3 install jupyter

- end virtualenv

        deactivate

### Add bash alias for virtualenv activation
    cd
    gedit .bashrc

- add at the end of `~/.bashrc`

        # Custom alias'es
        if [ -d ]; then
          alias pyenv='source /home/msonntag/Chaos/software/pyvirtualenv/pymain/bin/activate'
          alias pyenv3='source /home/msonntag/Chaos/software/pyvirtualenv/py3main/bin/activate'
        fi


### Activator
- DL from [homepage](https://typesafe.com/get-started).
- follow [install instructions](https://www.playframework.com/documentation).


### Install and setup everything required to build bootstrap
- get ruby(required for jekyll), jekyll and rouge (both required for grunt)

        sudo apt-get install ruby ruby-dev make gcc
        sudo gem install jekyll
        sudo gem install rouge

- get latest version of npm from [here](https://nodejs.org/)
- Create version independent link; add version independent link to $HOME/.profile
    (don't forget to `source` afterwards), e.g.:

        if [ -d "$HOME/work/software/nodejs" ] ; then
            PATH="$PATH:$HOME/work/software/nodejs/bin"
        fi
- //NOTE: NEVER EVER USE THE FOLLOWING COMMAND IT WILL BREAK NPM! (if not run from ~$ if I read correctly but who knows)

        sudo npm install npm -g

- get grunt (required for bootstrap)

        sudo npm install -g grunt-cli

- if required, install the following:

        sudo apt-get install nodejs nodejs-dev node-less


### Install Virtual box
    sudo apt-get install virtualbox virtualbox-dkms

- copy existing virtual boxes
- start the virtual machine by terminal, lock to launcher
- File -> Machine -> Add -> add virtual box
- Check settings: System - Base memory: 4096MB; Shared Folders: Win_Share (/home/msonntag/VirtualBox VMs/WinShare, auto-mount, full access)


### Install IRC client
    sudo apt-get install hexchat

- channels gnode@freenode, ccc@blafasel
- Settings -> preferences -> Monospace 12


### Install Postgres
        sudo apt-get install postgresql

### Install VLC
        sudo apt-get install vlc

### Install unity tweak to gain more control over ubuntu unity
        sudo apt-get install unity-tweak-tool


### Install Go
- download latest zip from golang.org
- install according to golang.org/doc/install
- unpack to directory of choice e.g. /usr/local or in our case $HOME/work/software

        tar -C $HOME/work/software -xzf go$VERSION.$OS-$ARCH.tar.gz

- in case of /usr/local installation add to /etc/profile

        export PATH=$PATH:/usr/local/go/bin

- in case of custom path installation add to $HOME/.profile
GOPATH is an additional directory, where go looks for installed packages

        export GOROOT=$HOME/Chaos/software/go
        export GOPATH=$HOME/Chaos/work/go-packages
        export PATH=$PATH:$GOROOT/bin
        export PATH=$PATH:$GOPATH/bin

- re-load $PATH

        source $HOME/.profile

### DL and install Visual Studio Code
- Download latest visual studio code
- Unpack it to directory of choice e.g. $HOME/work/software
- Add Launcher:
- create .desktop file in hidden ~/.local/share/applications folder

            gedit $HOME/VSCode.desktop &

- add following statements to file, change where required:

        [Desktop Entry]
        Version=1.6
        Type=Application
        Name=Visual Studio Code
        Icon=/home/msonntag/work/software/VSCode-linux-x64/resources/app/resources/linux/code.png
        Exec="/home/msonntag/work/software/VSCode-linux-x64/code"
        Categories=Development;IDE;
        Terminal=false

- move file to hidden folder

        sudo mv $HOME/VSCode.desktop $HOME/.local/share/applications/VSCode.desktop

- start, install add ons by F1 ... ext inst ... select add on
- install go, PowerShell, markdown linter

- on the commandline install all recommended go-tools according to [this](https://marketplace.visualstudio.com/items?itemName=lukehoban.Go).

        go get -u -v github.com/nsf/gocode
        go get -u -v github.com/rogpeppe/godef
        go get -u -v github.com/golang/lint/golint
        go get -u -v github.com/lukehoban/go-find-references
        go get -u -v github.com/lukehoban/go-outline
        go get -u -v sourcegraph.com/sqs/goreturns
        go get -u -v golang.org/x/tools/cmd/gorename
        go get -u -v github.com/tpng/gopkgs
        go get -u -v github.com/newhook/go-symbols
        go get github.com/derekparker/delve/cmd/dlv
        echo $GOPATH

### Install the following Go packages at the $GOPATH folder:
    go get -tags "nomymysql nomysql nosqlite3" github.com/CloudCom/goose/cmd/goose
    go get github.com/golang/lint/golint
    go get github.com/GeertJohan/fgt

- coveralls

        go get github.com/mattn/goveralls
        go get golang.org/x/tools/cmd/cover

- dependencies

        go get github.com/jmoiron/sqlx
        go get github.com/lib/pq
        go get gopkg.in/yaml.v2
        go get github.com/pborman/uuid
        go get golang.org/x/crypto/bcrypt
        go get github.com/docopt/docopt-go


## DL and install Protege
- add Launcher:
- create .desktop file in hidden ~/.local/share/applications folder
- add following statements to file, change where required:

        [Desktop Entry]
        Version=5.0
        Type=Application
        Name=Protege
        Exec="/home/msonntag/work/software/protege/Protege_5.0_beta/run.sh"
        Categories=Development;IDE;
        Terminal=false


## DL and install the latest Intellij IDEA Ultimate
- add Launcher:
- create .desktop file in hidden ~/.local/share/applications folder
- add following statements to file, change where required:

DL and install the latest Intellij IDEA Community
add Launcher:
- create .desktop file in hidden ~/.local/share/applications folder
- add following statements to file, change where required:

## DL and install Intellij PyCharm Community
- add Launcher:
- create .desktop file in hidden ~/.local/share/applications folder
- add following statements to file, change where required:

        [Desktop Entry]
        Version=1.0
        Type=Application
        Name=PyCharm Community Edition
        Icon=/home/[path]/pycharm-community-4.5.1/bin/pycharm.png
        Exec="/home/[path]/pycharm-community-4.5.1/bin/pycharm.sh" %f
        Comment=Develop with pleasure!
        Categories=Development;IDE;
        Terminal=false
        StartupWMClass=jetbrains-pycharm-ce

- change shortcut for closing subwindow to ctrl+w (settings, keymap, search for close tab)
- find more pycharm shortcuts [here](https://www.jetbrains.com/pycharm/help/configuring-keyboard-shortcuts.html).


## Update Ubuntu settings
- System settings -> Appearance ... enable workspaces
- System settings -> Appearance ... set icon size to small
- System settings -> Brightness & Lock ... set Turn off screen to 30min
- System settings -> Display ... Sticky edges ... off
- System settings -> Keyboard ... sound & media
- right click mp3/m3u -> properties -> open with -> VLC
- Open Nautilus/Files -> Edit -> Preferences -> Views -> List view
- Display shortcuts when in desktop view: keep super pressed


Uni specific
============

printer setup (hp p2015):
- add
- network printer
- HP LaserJet P2015 Series - should be the first entry
- use AppSocket/HP Direct ... should be done


## Set up GCA-Web

- get activator-1.3.7-minimal.
- create `/home/[user]/bin`; create link to `.../activator-1.3.7-minimal/activator` in `.../bin/`.
- restart or at least `source ~/.profile`.
- if exists, nuke `GCA-web` folder.
- clone GCA-web (add upstream)
- Open IDEA -> New -> Project from existing source -> Select clone -> create project from existing sources
    -> Keep name the same -> Overwrite .idea -> Do not select any pre-compiled source files -> finish
- Open Project Structure -> Project -> Select Java version -> Select Project language level 8 -> Apply
- Open build.sbt -> "Import project" (takes a while) -> Follow all sbt import Dialogs
- Should have imported 2 modules by the end of the process.
- At the shell to start the project use (this will take some time the first time):
    activator run
- Run all tests to populate database
- Add application run in IDEA ... Run -> Edit Configurations -> + -> Play 2 App -> Add name -> OK


## Set up NIX

- clone NIX, set upstream, fetch and rebase
- set up required folders for build

        cd nix
        mkdir build
        cd build

- set up build for nix

        cmake ..
        make -j 4 all

- set up test

        ./TestRunner

- run test, should complete without fail

        ctest

- build and install nix

        sudo make install


## Set up nixpy

- move to folder where nixpy should be imported
- clone files from github to local repository
- set up local repositories (if required)
- check recognised remotes first:

        git remote -v

- add upstream if required

        git remote add upstream git@github.com:G-Node/nixpy
        git fetch --all

- check existing libraries

        pkg-config --libs --cflags nix

- build nixpy with python2:

        pyenv
        python setup.py build
        python setup.py install

- manually remove all .egg folders and files
- setup documentation

        python setup.py build_sphinx

- deactivate python2 environment

        deactivate

- build nixpy with python3:

        pyenv3
        python3 setup.py build
        python3 setup.py install

- deactivate python3 environment

        deactivate


## Set up python-odml

- install dependencies

        sudo apt-get install libxml2-dev libxslt-dev

- clone python-odml, set upstream, fetch and rebase
- install python-odml for python2

        pyenv
        python setup.py install
        deactivate

- install python-odml for python3 ... still leads to errors.

        pyenv3
        python3 setup.py install
        deactivate


## Set up python-neo
- clone python-neo, set upstream, fetch and rebase from tag "api-break"
- install python-neo for python2

        pyenv
        python setup.py install
        deactivate

- install python-neo for python3

        pyenv3
        python3 setup.py install
        deactivate


## Set up python-neo-nixio
- clone python-neo-nixio, set upstream, fetch and rebase
- install python-neo-nixio for python2

        pyenv
        python setup.py install
        deactivate

- install python-neo-nixio for python3
        pyenv3
        python3 setup.py install
        deactivate


## Set up python-odml2
- clone python-odml2, set upstream, fetch and rebase
- install python-odml2 for python2

        pyenv
        python setup.py install
        deactivate

- install python-odml2 for python3

        pyenv3
        python3 setup.py install
        deactivate


# Win:
    notepad++
    chocolatey
    putty
    winscp
    java JDK
    python jdk
    git
    maven
    idea community
    idea pycharm

- Add python to path variable
(ctrl+x -> Systemsteuerung -> System -> Erweiterte Einstellungen -> Umgebungsvariablen -> add to path variable)
- install numpy (python -m pip install numpy)
- install scipy (python -m pip install scipy)

- install github for windows
- dl protege
- dl hdfview -> extract -> change paths in bin/hdfview.bat


Regarding Windows Installation / refresh / Linux Dual boot
- [Install Win from OEM key](http://superuser.com/questions/697253/clean-install-windows-8-1-or-windows-8-from-oem-key/900235#900235)
- [Refresh media](http://windows.microsoft.com/en-us/windows-8/create-reset-refresh-media)
- [Dual Boot](https://wiki.ubuntu.com/Touch/DualBootInstallation)


# Old stuff:
## Install IRC Chat with empathy (but probably skip this, empathy sucks)

        sudo apt-get install account-plugin-irc

- empathy: rooms, join, gnode


## Install LaTex and additional fonts

        sudo apt-get install texlive-full texlive texlive-doc-de texlive-latex-extra texlive-fonts-extra
        // update installed stuff (fonts, etc) for latex
        sudo texhash


## Install Gedit latex plugins
        sudo apt-get install gedit-latex-plugin

## JavaFX Scene Builder
- use software center, copy .desktop from /opt/JavaFXSceneBuilder2.0/ folder
- or get it from [here](http://www.oracle.com/technetwork/java/javase/downloads/sb2download-2177776.html).

## Install Idea 13 ultimate
- DL Idea 13 ultimate
- install Idea 13 in folder programs using university license key
- modify Idea settings using Java G_Node.xml file from adrian
- create file idea.desktop in ~./local/share/applications with content:

        [Desktop Entry]
        Type=Application
        Name=IntelliJ IDEA
        Comment=Integrated Development Environment (Version 13.1 EAP)
        Icon=/home/[path]/idea-IU-135.1289/bin/idea.png
        Exec=/home/[path]/idea-IU-135.1289/bin/idea.sh
        Terminal=false
        Categories=Development;IDE;Java;

- set up git as version control tool in idea

## Install python and python libraries

### Install python library installation tool
        sudo apt-get install python-pip python-pip3

### Install python libraries
        sudo apt-get install python-dev python-setuptools python-numpy python-sphinx python-h5py python-matplotlib python-scipy python-rdflib python-magic
        sudo apt-get install python3-dev python3-setuptools python3-numpy python3-sphinx python3-matplotlib python3-h5py python3-scipy python3-yaml

### Upgrade pip in case it was already installed
        sudo pip install --upgrade pip

### Install additional pyhton packages
        sudo pip install quantities coveralls cpp-coveralls mock==1.0.1
        sudo pip3 install quantities coveralls cpp-coveralls mock

### Install python packages (futures for python 2to3, alabaster for documentation)
        sudo pip install future alabaster


### Install neo dependencies
        sudo apt-get install python-numexpr python3-numexpr
        sudo pip install tables
        sudo pip3 install tables


- REPLACE the following with sole installation of jupyter - check if its best to install solely via pip and NOT via apt-get
- To see which kernels and where they are installed, check `jupyter kernelspec list`


## Install ipython and ipython notebook
        sudo apt-get install ipython ipython-notebook ipython3 ipython3-notebook

## Upgrade ipython notebook to jupyter
        sudo pip install -U jupyter


If something does not work with the jupyter notebook, check this first
for [troubleshooting](http://stackoverflow.com/questions/28831854/how-do-i-add-python3-kernel-to-jupyter-ipython/28840041#28840041).