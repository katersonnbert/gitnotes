IDEA:
=====

Under Linux, if you encounter the following error message upon startup:
        IBus prior to 1.5.11 may cause input problems. See IDEA-78860 for details

add the following line to the idea.sh startup shell script right after the shebang line:

        export XMODIFIERS=""

Finds details how to create git ssh keys [here](https://help.github.com/articles/generating-ssh-keys/).


# Setting up IDEA

## Set line ending:

Line endings for your current system (Mac: \r, Linux: \n, Win: \r\n) can be set at the (right) bottom of the
IDEA window next to the line:column display.

## Display whitespace:

For one file only:

    View > Active Editor > Show Whitespaces

Globally:

    Settings > Editor > General > Appearance > Show whitespaces


## Disable IntelliJ Starred (Package) Imports?

    File > Settings > Editor > Code Style > Java > Imports > Class count to use import with '*'

Find details [here](http://stackoverflow.com/questions/3587071/disable-intellij-starred-package-imports).


## Set tab size:
    File > Settings > Editor > Code Style > [Language] > Tabs and Indents


# Update IDEA
- make sure that a launcher for idea is available, if not create one using the following template.
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
- move the created .desktop file to hidden folder ~/.local/share/applications
- manually edit the properties (`properties > permissions > execute`) to make it executable
- draw it onto the quick launcher bar.


## Community edition
- DL newest version from Intellij
- unzip to folder of joice
- change properties of quick launcher to `.../[new idea directory]/bin/idea.sh`.

## Ultimate edition
- same as community edition
- at first open, activate by providing Intellij account data.


# IDEA ShortCuts:
    alt + enter ... additional options, complementation, etc
    alt + insert ... adding constructors, etc
    ctrl + q ... documentation for selected item
    shift + F6 ... rename stuff
    ctrl + alt + o ... autorefine import statements
    ctrl + alt + B ... go to implementation
    shift + esc ... hide active window


# Shortcuts in IDEA 15:
Find details [here](https://dzone.com/refcardz/intellij-idea-update)

    Back to editor          Esc
    hide subwin             shift + esc
    Toggle projTree win     Alt + 1
    Toggle run window       Alt + 4
    Toggle console          Alt + F12
    move to method          ctrl + F12 / alt + 7
    run app                 ctrl + F10
    Scratch file            ctrl + alt + shift + insert     (will affect file history! which sucks)
    Search everywhere       ctrl + shift + a                (also works for reopening projects)
    switch subwin/files     ctrl + tab
    recent files            ctrl + e
    zoom                    ctrl + mouse-wheel
    jump to next method     alt + up/down

    copy line               ctrl + d
    comment line            ctrl + numpad /

    move line/method        ctrl + shift + up/down
    join line               ctrl + shift + j                (move last line to cursor position)
    (smart) split line      alt + enter                     (intellij senses context - will present split
                                                                option if appropriate)
    language injection      alt + enter                     (IDEA context sense - will present
                                                                language inj option if appropriate)
    check RegExp            alt + enter                     (if language injection regular expression: alt
                                                                + enter shows testwindow)
    create test class       alt + enter                     (if positioned over a class name, alt + enter
                                                                will add the option to create a testfile)
    add test method         alt + insert
    fold/expand code block  ctrl + .
    refactor                ctrl + F6                       (refactors variables, methods and classes)
    definition peak         ctrl + shift + i

    close file              ctrl + w                        (custom key map)
    select word             alt + a                         (custom key map)
    show file history       alt + '                         (custom key map)


## Postfix completion:

    var.for         creates     for (int value : var) {}
    var.fori        creates     for (int i =; i < var; i++) {}
    var.format      creates     String.format(var, )
    var.if          creates     if (var) {}
    var.nn          creates     if (var != null)
    var.par         creates     (var)
    var.sout        creates     System.out.println(var)
    var().try       creates     try {var();} catch (Exception e) {e.printStackTrace();}


# Using IDEA

## Using commandline inputs:
- `Run > Edit configurations`


# Specific problems with projects in IDEA

## How to work with CSS resource files
- all files that are not ".java" have to be in a folder that's marked as resource in IDEA.
If an fxml file is supposed to be found, the folder it resides in has to be marked as resource explicitly.
- CSS files always have to be in the resources folder!
- the same applies to files that are referenced by the css file


## How to create / add a Scala+Play project
Get plugins for play and scala:

        Settings > plugin > Install JetBrains plugin

- search for scala - install `scala` plugin
- search for play - install `play x.x support` plugin
- Close IDEA and open new scala/play2 project
- In Settings/IDE `Settings > Scala`: change JVM SDK to latest java version


## Problems with building a project after IDEA update:
- move to main old project folder, remove old dependencies:

        rm target project/target project/project/ .idea/ *.iml -rf

- close project in IDEA and CREATE new scala + play 2 project, not import!
- select project folder


# Building a java project:
- After setting up a new project, set up the project structure
- use maven projects
- set git as version control for project (settings - version control - add paths and stuff)
- with each new class and other stuff add to git



MAVEN
=====

# How to build a java project with maven

- check if java version is 1.8 or higher:

        java -version

- check if maven version is 3 or higher:

        mvn -v

- Move to a directory where the project can be built and installed:

        cd work
        mkdir tmp
        cd tmp

- Clone project to be built from github to local build directory
- Use an optional directory name, otherwise the directory name of the git project will be used

        git clone git@github.com:G-Node/gndata-editor.git testBuild

- move to main project folder e.g. testBuild

        cd testBuild

- Build project

        mvn clean compile package install

- Start application

        java -jar target/gndata-*.jar


# Setup a Java MAVEN project with IDEA

- requires: latest java JDK installation, maven installation
- check maven install for more details

## Windows:
maven requires the global variable JAVA_HOME, the maven bin directory has to added to the windows path variable

- create JAVA_HOME:

        System > advanced system settings > environment variables

- here

        add new > JAVA_HOME + main directory of the java jre

- add maven directory, again:

        System > advanced system settings > environment variables

- here modify system variable "path" and add the path to the maven bin directory at the end.

- Find more details here:
    - [Edit system path](http://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access/)
    - [Java home in Windows](https://confluence.atlassian.com/display/DOC/Setting+the+JAVA_HOME+Variable+in+Windows)


# Create a new Maven project:

Find more details [here](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)

- Maven is a project setup tool ... it sets up project structure dependent on the archetype you select for a new project.
- To create a new maven project, identify which archetype you want to use (google maven archetypes, duh).
- Move to the directory where the new project should be created

        mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

Description:
- `archetype:generate` ... creates a maven project based on an archetype
- `-DgroupId=org.g_node` ... namespace the project will be created in. will create a package with this namespace.
        use a unique one, if you want to publish your package somewhere else
        [xxx] better description. in case of g-node projects always use org.g_node
- `-DartifactId=` ... name of the project. will create `src/main/java/[name of the project]`
- `-DarchetypeArtifactId=` ... the archetype that is used to create the project structure
        ... check documentation for different archetypes.


To use IDEA with a Maven Project go to the start screen:
- Import project
- select path
- create maven project
- follow wizard
- Once that's done check

        File > Project Structure > Project

- set Project language level to 8 - Lambdas

- Add the following to the pom file:

        <build>
          <plugins>
            <!-- some standard maven plugins for compilation and jar creation -->
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <version>3.1</version>
              <configuration>
                <source>1.8</source>
                <target>1.8</target>
              </configuration>
            </plugin>
          </plugins>
        </build>


If you made any mistakes during the import process delete the following stuff from the main project folder and import again:
folders:

    - target
    - out

    .idea (hidden)

    files (from all subfolders):
    *.iml



## Create new IDEA Maven project with an existing github repository.

- Create new IDEA Project with groupID and artifactID - e.g. "org.g_node" and "tag-notes"; avoid "-" in groupID
- Close IDEA
- Create github repository with the same name as the artifactID
- rename artifactID folder to tmp
- git clone repository to local work directory with the artifactID as folder name
- copy all files and folders from the tmp directory to the new git folder. NOTE! also copy all HIDDEN files at this step!
- open IDEA again.
- `File > Project Structure > Project`: set Project language level to `8 - Lambdas`
- add all required files from another java maven project, update pom file.
- get IDEA and git to play nice:

        File > Settings > Version Control > add project root path, apply.

- add repository to: travis; appveyor; coveralls
- add badges to readme.md
