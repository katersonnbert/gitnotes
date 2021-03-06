Continuous integration
======================


Travis CI with Github
======================

Find a tutorial [here](http://docs.travis-ci.com/user/getting-started/).

- sign in to travis CI with the github account
- add organization
- click add repository
- flick additional repository to active that is supposed to be built by travis
- add .travis.yml file containing all build information

- to show the current status of a travis branch
    e.g. in the github readme [check here](http://docs.travis-ci.com/user/status-images/)

- add email [notifications via yaml](http://docs.travis-ci.com/user/notifications/)

- to get the link to the badge:
    move to travis-ci.org/[username]/[project name]
    left click the badge next to the title, select markdown.


Travis CI specifics:

[Validate a Travis .yaml](http://lint.travis-ci.org/)

There are no 32 bit environments under [Travis CI](https://github.com/travis-ci/travis-ci/issues/986)

[OSX does not support python builds at the moment (07.12.2015)](https://docs.travis-ci.com/user/languages/python)



AppVeyor with GitHub
====================

[Appveyor](http://www.appveyor.com/)

- sign in with github account
- use free open source project
- authorize with github pw
- add wanted project
- add appveyor.yml file

More information about the appveyor yaml file:
[Appveyor](http://www.appveyor.com/docs/appveyor-yml)
[Appveyor Maven](http://www.yegor256.com/2015/01/10/windows-appveyor-maven.html)

- change email notifications via UI

- to show the current status of an appveyor branch e.g. in the github readme go to the settings page of the respective project e.g.:
    (https://ci.appveyor.com/project/mpsonntag/crawler-to-rdf/settings/badges)
    copy markdown code



MAVEN
=====

build project locally

    mvn clean compile package install



JACOCO MAVEN plugin
===================

- Jacoco is a plugin that provides information about code coverage by unit tests.
- Its included in the pom.xml file
- The results can be found at [project]\target\site\jacoco\index.html


!!!!!!! DO ME
To get jacoco test results:
- remove old build from testfolder
- copy all files except "target" folder but including hidden files to the testfolder
- run from the project root:

    mvn clean compile test jacoco:report



Coveralls with Travis CI & GitHub
=================================

[Coveralls](https://coveralls.io)

- sign in with github repository
- authorize application
- go to repos
- turn repository of choice on

- add the following line to the travis yaml file:

        after_success:
            - mvn clean test jacoco:report coveralls:jacoco

- add continuous maven plugin for coveralls to project pom.xml (requires jacoco maven plugin). e.g.

        <plugin>
            <groupId>org.jacoco</groupId>
            <artifactId>jacoco-maven-plugin</artifactId>
            <version>0.7.1.201405082137</version>
            <executions>
                <execution>
                    <id>prepare-agent</id>
                    <goals>
                        <goal>prepare-agent</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>

          <plugin>
            <groupId>org.eluder.coveralls</groupId>
            <artifactId>coveralls-maven-plugin</artifactId>
            <version>2.2.0</version>
          </plugin>

- the pom.xml also requires the following properties to state the source encoding for coveralls:

        <properties>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        </properties>


- to show the current status of a coverall e.g. in the github readme go to the project coverall page,
there is a badge url at the top of the page


Findbugs, Checkstyle, PMD:
==========================

[Findbugs](http://www.petrikainulainen.net/programming/maven/findbugs-maven-plugin-tutorial/)

    mvn -Dpmd.printFailingErrors=true pmd:check checkstyle:check findbugs:check


Create Javadoc:
===============

[Javadoc](https://newcircle.com/bookshelf/java_fundamentals_tutorial/javadoc)

This is how it works:

    javadoc -private -d [path] [sourcepath] -subpackages [list of packages]

- `-private` ... includes everything (packages, classes, class members) into the documentation
- `-d` ... output directory
- `-subpackages` ... required to add every sub package specifically which sucks.

Example: Don't mind the warnings:

    javadoc -private -d target/javadoc/ src/main/java/org/g_node/*.java -subpackages src/main/java/org/g_node/crawler/*.java src/main/java/org/g_node/crawler/LKTLogbook/*.java


With the javadoc maven plugin:

      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-javadoc-plugin</artifactId>
        <version>2.9.1</version>
        <configuration>
          <aggregate>true</aggregate>
          <show>private</show>
          <nohelp>true</nohelp>
          <header>crawler-to-rdf, ${project.version}</header>
          <footer>crawler-to-rdf, ${project.version}</footer>
          <doctitle>crawler-to-rdf, ${project.version}</doctitle>
        </configuration>
      </plugin>

    mvn clean javadoc:javadoc


Eierlegende wollmilchsau:

    mvn -Dpmd.printFailingErrors=true pmd:check checkstyle:check findbugs:check clean compile test javadoc:javadoc jacoco:report
