Linux and using the Bash (bourne again shell)
===============================

##  Using the shell

Baby steps:
1) get someone to show you how to open a shell and execute commands
2) get to know the file system and how to navigate through it (see explanatory tutorial above)
3) get to know the basic shell commands described below
4) script away

### Opening a shell
Under Ubuntu Linux a terminal can be opened by using `Ctrl + Alt + T`.


###  More resources:

Understanding the unix file system (you should really read this):
- http://www.december.com/unix/tutor/filesystem.html

and (you could read this)
- http://linuxcommand.org/lts0040.php

and (if you are really interested)
- https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard

Shell Tutorials (you can work through this if you want to know more about using the shell)
- http://linuxcommand.org/learning_the_shell.php
- https://linuxacademy.com/blog/linux/conditions-in-bash-scripting-if-statements/

## Navigating the filesystem:
When you first open your shell you will see something like this:
    
    [chris@troll ~]$

This means, that you have logged into a linux network with username "chris",
that you currently use the computer named "troll" and are currently within the space,
the filesystem has reserved as your workspace (indicated by "~").

In a more abstract way will always see the information like this:

    [username@computername current_folder]$

You can navigate through the filesystem by using the "cd" (change directory) command right after the "$" sign.
Work yourself through the navigation introduction provided with the links above.


## Linux nice to knows:

- Special characters which are not allowed in file- or folder names:

    ` /, .., ~, *, ?, >, <, |, \`

- Linux is case sensitive!
    `Filename.txt` is not the same as `filename.txt`

- Try to avoid blanks within file- or folder names, always use underscore or minus, especially,
if you create folders or files using the filebrowser!
        e.g.

        do not use:
        project computational systems biology
        
        use:
        project_computational_systems_biology

- Use the auto-complete function provided by the tabulator key:
    example:

        [username@computername ~]$cd /h
        
    hit "tab" once, it should auto-complete your entry to

        [username@computername ~]$cd /home/
        
    if you hit "tab" twice, it will display the contents of the directory, without disrupting your command entry
        
        [username@listeria ~]$ cd /home/
        admin/ apps/  conf/  edu/   proj/  user/
        [username@listeria ~]$ cd /home/

    if there are more than one files or directories with the same starting letters (e.g. /home/ or /hello/) you will have to enter the next letter, before autocomplete will work
        
        [username@listeria ~]$ cd /h    
    "tab" will  not work
        
    but:
    
        [username@listeria ~]$ cd /he

    "tab" returns:
    
        [username@listeria ~]$ cd /hello/

- Get help! : either by using "man" (manual) command:

        [username@computername directory]$man [command]
        
        e.g. [chris@troll ~]$man cp
        
    or by using `--help` commandline option:

        [username@computername directory]$[command] --help

        e.g. [chris@troll ~]$cp --help

- Most of the command line programs displaying the contents of text files can be ended by simply pressing "q".

- How to end a running program:
        `ctrl + c`

- How to stop a running program (process will be on hold in the background):
        `ctrl + z`

- How to kill a running program (process will be terminated):
        `ctrl + d`
        
- No paper basket! If you remove files or directories using the shell, there is no easy recovery!


# Basic Bash commands:

- `ls`        ... display files and directories within your current directory
- `ls -l`     ... like ls, but display additional information
- `ls -a`     ... like ls, but also display hidden files and directories
- `ls -l -a`  ... combination of `ls -l` and `ls -a`
- `ls ../Foldername` ... display the contents of directory "Foldername" residing on the same
hierarchical layer as the folder we are currently residing in
- `ll` ... like ls -l
- `pwd` ... print working directory, prints the complete path from root until the directory we are currently residing in

        e.g. [chris@troll papers]$pwd
             /home/users/chris/papers/
             [chris@troll papers]$

- `cd [path]` ... change directory; move within the filesystem from your current directory to another directory specified in your [path]

        e.g. [chris@troll ~]$cd /home/user/franz/

- `cd ..`   ... change from the current directory to the directory directly above.

- `mkdir foldername` ... create folder "foldername" at your current position within the filesystem

        e.g. [chris@troll papers]$mkdir cell_papers

- `rmdir foldername` ... remove folder "foldername" from the filesystem. will only work, if the folder is empty.

        e.g. [chris@troll papers]$rmdir cell_papers

- `rm filename` ... remove file "filename" from the filesystem.

        e.g. [chris@troll papers]$rm 2011_Science_2282772.pdf

- `rm foldername -r` ... remove folder "foldername" from the filesystem including all files and folders it contains.

        e.g. [chris@troll work]$rm papers -r

- `cp path1/source path2/target` ... copy file "source" residing at location "path1" to file "target"
residing at location "path2"

        e.g. [chris@troll work]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf /home/users/chris/work/papers/science/2011_Science_2282772.pdf

- `cp path1/source ./target` ... copy file "source" residing at location "path1" to file "target" at the current location

        e.g. [chris@troll science]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf ./2011_Science_2282772.pdf

will copy the file 2011_Science_2282772.pdf from location /home/user/chris/work/papers/ to location /home/users/chris/work/papers/science/

- `cp path1/source .` ... copy file "source" residing at location "path1" to the current location, keeping the same filename.

        e.g. [chris@troll science]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf .
will copy the file 2011_Science_2282772.pdf from location /home/user/chris/work/papers/ to location /home/users/chris/work/papers/science/

- `mv [Pfad-Quelle]/[Filename] [Pfad-Ziel]` ... same as "cp" command, but moves the file from one location to the other, deleting the original file.

- `echo text` ... prints "text" onto the screen

        e.g. [chris@troll work]$echo hurray for icecream!
        
- `echo text > filename.txt` ... saves "text" into file "filename.txt" which will be created at the current filesystem location

        e.g. [chris@troll work]$echo hurray for icecream! > important.txt

- `cat filename` ... prints the contents of file "filename" onto the screen.

        e.g. [chris@troll work]$cat important.txt

- `less filename` ... displays the contents of file "filename" in the screen, contents are scrollable by using "up" and "down" keys. end this by pressing "q"

        e.g. [chris@troll work]$less important.txt

- `history` ... list of all commands executed within this terminal

        e.g. [chris@troll ~]$history

- `exit` ... close the shell

        e.g. [chris@troll ~]$exit

- `man [program]` ... manual of a command line program. Displays a brief description and all command line options.

        e.g. [chris@troll ~]$man ls


## Further basic Bash command line options:

- `[command] > [filename]` ... will write the output of a specific `[command]` to a specific file.
Note, that an already existing file with the same filename at the same location will be replaced!

        e.g.    [chris@troll work]$echo hurray for icecream! > important.txt
                [chris@troll work]$less important.txt
                [chris@troll work]$echo another hurray for schnitzel! > important.txt
                [chris@troll work]$less important.txt

- `[command] >> [filename]` ... will append the output of a specific `[command]` to a specific file.
If the file does not exist yet, it will be created.

        e.g.    [chris@troll work]$echo chicken >> shopping_list.txt
                [chris@troll work]$less shopping_list.txt
                [chris@troll work]$echo onions >> shopping_list.txt
                [chris@troll work]$echo lemons >> shopping_list.txt
                [chris@troll work]$echo olive oil >> shopping_list.txt
                [chris@troll work]$echo rosemary >> shopping_list.txt
                [chris@troll work]$less shopping_list.txt

- `[command] &` ... start a program running in the background, getting back the shell.

        e.g.    [chris@troll work]$firefox &

- `[command]` ... start a program

        e.g.    [chris@troll work]$firefox

- `ctr + z`         ... stop the execution of the program and keep it on hold in the background
- `bg`              ... restart the program, but keep it running in the background (bg)

                [chris@troll work]$bg

- `fg`              ... if you have a program running in the background, get it back by fg (foreground)

        e.g.    [chris@troll work]$fg

- `command_1 | command_2` ... | ... "pipe". executes "command_1", directs the output of this command not to the screen, but the second command "command_2". Only then the output of "command_2" will be printed onto the screen.

- `dd`          ... create a file with a specific size

        # create 10MB file named 'out.txt' filled with zeros 
        e.g. dd if=/dev/zero of=out.txt bs=1M count=10

### Formatting commandline output:

Output from commandline programs can easily be formatted to display as a table
by piping it to the `column` command e.g.:

    cat /etc/passwrd | column -t -s :


### File content handling

- replace spaces with tabs

        cat textfile.txt | tr ':[space]:' '\t' > out.txt

- convert a file to upper or lower case

        cat textfile.txt | tr a-z A_Z > out.txt


### Displaying disc space

    df -h

## User and group handling

Unix systems feature groups and users to handle any access permissions.

### Users

Every user belongs to a user group.

All registered users including their userID and their groupID can be listed via

    cat /etc/passwd

To display all active/available users:

    users

- add a new user w/o a home directory:

    sudo useradd [new_username]

- add a new user and create a home directory and a password for this user:

    sudo useradd -m [new_username] -p [password]

- modify the name of a user

    usermod -l new_username old_username

- check the id of a user and the groups a user is assigned to:

    id [username]

- modify the id of a user; use IDs 1000+, ids must be unique.

    usermod -u [newID] [username]

- add a user to an existing group

    useradd -g [groupID] [username]



### Groups

users can belong to groups, permissions can be given via groups 
to all users within a group.

All registered groups can be displayed via

    cat /etc/group

To display all active/available groups:

    groups

- add a group

    sudo groupadd [groupname]

- change groupID

    groupmod -g [newID] [group]


## Secure connection and keys 

### ssh (Secure SHell)

- Connect to another machine using secure shell (an encrypted connection to another computer within the network). 
- Once connected, the cpu of this machine will be used for all further actions.
- Makes sense to do large calculations on the server rather than on the local machine.
- Logging on to another machine will most likely require a password.

        ssh [remote machine]

        e.g.
        [chris@troll work]$ssh server4
        [chris@server4 ~]$

- Notice that the name of the machine in the bash shell has changed from `troll` to `server4`
- Use the command `exit` to close the ssh connection to a remote machine.

        e.g.
        [chris@server4 ~]$exit
        [chris@troll ~]$

### scp (Secure Copy)

- Use a secure connection to copy files from or to a remote machine

        scp [computername]:[remote_directory]/[filename] [current_directory]

        e.g.
        [chris@troll work]scp server4:/temp/work/* /home/user/chris/work/

### SSH keys

- For some secure connections, specific SSH keys are required e.g. to use the ssh option with github.
- if you are not familiar with PGP, read up on your trusty wikipedia page.
- generate ssh keys:

        ssh-keygen
        enter a name (without spaces!) e.g. id_rsa_gin_gnode_org
        enter a pw pr leave it empty
        // print public key
        cat id_rsa_gin_gnode_org.pub

- The created key pair can be found in a hidden `.ssh` folder in the home directory, if no other location has been 
    specified.

- To retrieve the SSH fingerprint of the public key use the following:

        ssh-keygen -lf [location and name of the public key]


### SSH Key Agent

Find a nice introduction [here](http://sshkeychain.sourceforge.net/mirrors/SSH-with-Keys-HOWTO/SSH-with-Keys-HOWTO-6.html) 
and laugh at the passphrase.

SSH keys reside in their folder, every time they are used, the passphrase has to be entered. 
To make them available to other services without having to point services to their proper keys and 
having to enter the pass phrase all the time.

- start an ssh-agent if it is not already started (might need one of the examples below to start properly)
    - BEWARE! when starting the agent, the environment will be reset, meaning any active agent
                is stopped, the new agent will not contain any keys, any keys previously added
                have to be added again! So when in doubt, run `ssh-add -l` first.

        eval "ssh-agent"
        eval "ssh-agent -s"
        eval `ssh-agent -s`
        or
        eval $(ssh-agent)

- add key to the agent e.g. the key created in example above and enter the keys pass phrase once and never again.

        ssh-add ~/.ssh/id_rsa_gin_gnode_org

- check which keys are currently managed by the ssh key agent

        ssh-add -l

- even if you have added the key to the ssh key agent, if the key with the corresponding passphrase 
    is not added to the linux keyring, it will prompt for the passphrase upon the first use of the 
    ssh key. If the option is used to store key and phrase in the keyring, this login will be
    done automatically and the prompt will not show up again, when the key is used.

- NOTE: as an important side note, when you have a lot of keys added to your agent, it will
  try all of them against a potential server you want to log in to. If the server has a low
  authentication failure tolerance, then you will be disconnected and potentially barred from
  new attempts for a while. Usually the message `Received disconnect from [IP address] port 22:2: 
  Too many authentication failures for [username]` is a dead giveaway. This can already happen
  when you have more than three ssh keys...
  You can debug this using the verbose setting on the ssh command e.g. `ssh -v username@some.server.com` 


### Even easier ssh access by using .ssh/config

As described above, having too many different ssh keys can lead to not being able to login somewhere.
Its even easier to tell the agent which key to offer to a specific server first to avoid that dilemma.

A nice tutorial for that can be found [here](https://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/).

- Just create a `config` file in the `.ssh` folder.
- Define a specific Host and point to the ssh key file that should be used.

    Host some.example.org
        IdentityFile ~/.ssh/id_rsa_for_example

- If you want to login at a specific port or with a different user than your current one:

Host other.site.com
    User root
    Port 4444
    IdentityFile ~/.ssh/id_rsa_for_site

## Setup user with sudo access on an ssh accessible server

Modified from [here](https://aws.amazon.com/premiumsupport/knowledge-center/new-user-accounts-linux-instance/).

- ssh to the server
- create new user (add password in the process)

        sudo adduser new_user

- [Optional] add user to the sudoer user group (after the user has been created)

        sudo adduser new_user sudo

- switch to new user, create .ssh folder and authorized key entry with the appropriate permissions for this user

        sudo su new_user
        mkdir /home/new_user/.ssh
        chmod 700 /home/new_user/.ssh
        touch /home/new_user/.ssh/authorized_keys
        chmod 600 /home/new_user/.ssh/authorized_keys

- open authorized keys with an editor e.g. and paste the required public key string into the file.

        vim /home/new_user/.ssh/authorized_keys
        "press i"
        "paste public key string"
        "press esc and type :x to save changes"

- [optional] change user password

        passwd

- Now the new_user should be able to ssh into the server.


## Even more useful Bash commands:

### wc (word count)

- counts lines, words and letters within a file

        wc filename 

        e.g.
        [chris@troll work]wc shopping_list.txt


### top (display active processes)

- Displays all processes running on the currently used machine.
- Exit by pressing `q`


### ps - report a snapshot of currently running processes / programs

        ps

        # list all processes, display in user oriented format
        ps aux
        # list all processes, sort by CPU usage
        ps aux | sort -rnk
        # list all processes, sort by Memory usage


### curl (commandline url)

Read up on the http protocol if you are not familiar with it. 

- commandline tool for sending a GET http request (GET is the default http request method for curl)

        curl "http://www.hntoplinks.com"

- include the http response headers: `-i`

        curl -i "http://www.hntoplinks.com"

- add a request header: `-H`

        curl "http://www.hntoplinks.com" -H "Accept: application/json" 

- use a different request method than GET: `-X`

        curl -X POST "http://somewhere.com?value=key"

- add a request body to the request: `-d`

        curl -X POST "http://somewhere.com" -d "value=key"

- add an http form body to the request: `-F`

        curl -X POST "http://somewhere.com" -F user[lname]=Karl

Find a very nice introduction to curl [here](http://conqueringthecommandline.com/book/curl).


### stat - file information

- print information about a specific file

        stat [filename]

### nmap - scan domain for open ports

- to check if a server is secure or has open ports, use `nmap`

        nmap [domain]

        # e.g.
        nmap example.com


### Compression / uncompression of files

- Use `gzip` or `tar` for compression related activities
- Please refer to the manual for details


### Programs for text file handling

- `sort` ... Sorts standard input then outputs the sorted result on standard output.
- `uniq` ... Given a sorted stream of data from standard input, it removes 
duplicate lines of data (i.e., it makes sure that every line is unique).
- `fmt` ... Reads text from standard input, then outputs formatted text on standard output.
- `pr` ... Takes text input from standard input and splits the data into pages with page breaks, 
headers and footers in preparation for printing.
- `head` ... Outputs the first few lines of its input. Useful for getting the header of a file.
- `tail -f` ... Outputs the last few lines of its input. Useful for things like getting the most 
recent entries from a log file.
- `tr` ... Translates characters. Can be used to perform tasks such as upper/lowercase conversions 
or changing line termination characters from one type to another (for example, converting DOS text files 
into Unix style text files).
- `grep` ... Examines each line of data it receives from standard input and outputs every line that contains 
a specified pattern of characters. Find some neat examples [here](http://www.panix.com/~elflord/unix/grep.html)
- `sed` ... Stream editor. Can perform more sophisticated text translations than tr.
- `awk` ... An entire processing language designed for filtering text files, 
more [here](http://www.vectorsite.net/tsawk.html)


## Root access by using `sudo`
[Introduction to sudo](https://www.linux.com/learn/tutorials/306766:linux-101-introduction-to-sudo)

In a nutshell `sudo` is the commandline way to grant a user root access for the following command 
e.g. when installing a software package. It will require a password that is associated with a user 
which has root access.

    sudo [command]
    [sudo] password for [username]:


## Installing packages by using apt (Advanced Packaging Tool)
[Quick apt-get overview](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool)

When working on Debian based Linux systems, apt-get is a convenient way to download and install software packages.
For most of the packages root access is required which means using `sudo apt-get [command]`.

- Update the index to be up to date with all the packages that can be installed via apt-get

        sudo apt-get update

- Install a package e.g. the terminator linux terminal, which is an awesome linux terminal

        sudo apt-get install terminator

- Install a package that is not handled via the normal apt-get package list e.g. the java8 distribution: 
This requires to manually add a repository to the apt-get list of installable packages, update the repository and the install the package.

        sudo add-apt-repository ppa:webupd8team/java
        sudo apt-get update
        sudo apt-get install oracle-java8-installer

- Print installed packages to terminal

        apt --installed list

        # grep to narrow down for specific packages
        apt --installed list | grep "somePackage"

- Display information for specific installed packages (requires exact package name)

        dpkg -s <packagename>

        # or
        dpkg-query -l <packagename>


## Using SFTP with the file browser (files/nautilus)
- ctrl + l
- enter `user@url` ... e.g. `nix@projects.g-node.org`
- enter pass phrase
 

## Finding where a program is installed

    which [program name]


## File operations

- `touch fileName` ... create file "fileName"
- `echo "text" > fileName` ... Overwrite file
- `echo "text" >> fileName` ... Add to file after existing content

## File permissions

- `chmod [options] [permissions] [filename]` ... change file permissions

### Change permissions to read, write and delete for everyone:

    chmod 777 filename

Find detailed description and examples [here](http://www.computerhope.com/unix/uchmod.htm)


## SymLinks (Symbolic links) to files or folders

### Create Symlink (symbolic link)
        ln -s [target-filename] [symbolic-filename]

This creates a symbolic link named `[symbolic-filename]` that points to `[target-filename]`
NOTE: ALWAYS use the whole path as `[target-filename]`, do not use relative terms.

        ln -s /home/currentUser/tmp/testLinks/hurra.txt ../hurraLink

### Delete Symlink
Symlinks can simply be deleted by rm. This will not touch the file the link points to.

        rm hurraLink

### Read a link

To show where a link actually leads to use:

        readlink [linkName]
        // e.g. using the link from above
        readlink hurraLink


## Linux System variables
- `$HOME` ... contains absolute path to the home folder
- `$PATH` ... contains all directories which are included when looking for an executable


## Important system folders in Linux:

- /proc ... [TODO]

###  Global searchpath variable $PATH:

- display all directories currently included in the searchpath:

        echo $PATH

- permanently add directories to `$PATH` by editing `/etc/profile` or `$HOME/.profile` by adding

        export PATH=$PATH:/[directory of choice]

- Symlinks for the operating system:

        /etc/alternatives

- Custom packages and libraries can usually be found in one of the following folders:

        /opt/
        /usr/local/include/
        /usr/local/lib/ & /usr/local/lib/pkgconfig/
        /usr/share/
        /home/[user]/bin/


## Adding custom executable paths in Linux:

- Set up a bin folder that is included in the Linux `$PATH` which contains all the links to starting programs.
- Check out this link for [more information](http://www.troubleshooters.com/linux/prepostpath.htm).
- Folders can easily be added to the beginning and the the end of the variable -
depending on where the application is first found, this will be executed.

Example:
In folder ~ (absolute /home/msonntag in our example)

    mkdir bin

Check if path has been automatically added by linux:

    cat $PATH

If not, then execute the following to recompile the .profile file:

    source .profile

Check path again

    cat $PATH

If its still not in there, then add it manually to the beginning of the `$PATH`:

    export PATH=/home/msonntag/bin:$PATH

Create symbolic links to executables in custom bin folder. As example add startup shell script of application activator
can be used to easily switch between different distributions of the same application

    cd bin
    ln -s /home/msonntag/work/software/activator-1.2.12/activator activator


## Find text in files using grep

- Find text "NoResultsException" in all files of all subfolders (-R) of the current folder (.)
and print the line number where the match has been found (-n).

        grep -Rn "NoResultsException" .


## netstat: check connections and available sockets
netstat - Print network connections, routing tables, interface statistics, masquerade connections, 
              and multicast memberships

- `netstat -p`      ... show all connections with their program name and pid
- `netstat -l`      ... check connections
- `netstat -lt`     ... check connections showing their local address
- `netstat -ltupn`  ... show all connections with their ports, program name and pid

## iptables: IPv4/v6 inspection and packet filter rules

Interesting to manage forwarding rules

    iptables --help

## List USB devices
    lsusb

- `lsusb` lists all connected usb devices
- `lsusb -v` does the same as above, but includes more details about the devices
- `lsusb -d [vendorID]:[deviceID] -v` gives a verbose description of just the specified device

## base64

base64 encoding from the commandline

    base64 <<< sometextstuff

base64 decoding from the commandline

    base64 -d <<< c29tZXRleHRzdHVmZgo=

The base64 RFC with the nittygritty details can be found [here](https://tools.ietf.org/html/rfc4648#section-5). 


## Bashrc
- The hidden `.bashrc` file is a script that is executed, whenever a new terminal session is started.
- The file contains configurations for the terminal session. An example would be the shorthand `ll` instead of `ls -l`
- There are two main bashrc files
- The first is found in `/etc/bash.bashrc`, this one applies to all shell sessions
- The second one is found in `/home/[user]/.bashrc`. This one applies only to non-login shell sessions.


## Linux server crash course:

- client and server are both processes, that communicate with each other via SOCKETS.
- processes can only communicate, if they are in the same ADRESS DOMAIN and have the same SOCKET TYPE.
- the two main different address domains are the UNIX DOMAIN (processes live in a common file system) and the INTERNET DOMAIN
(processes live on two hosts anywhere in the internet)
- the address of a socket in the unix domain is a CHARACTER STRING entry in the file system
- the address of a socket in the internet domain is the internet adress of the host machine which is the IP ADDRESS (unique 32bit address).
- Each internet socket address needs a port number on its host (16bit unsigned integer).
- There are port numbers reserved for Unix standard services, port numbers above 2000 are available.
- The mainly used socket type is the STREAM SOCKET. Stream sockets TCP (transmission control protocol).

Find a nice introduction [here](http://www.linuxhowtos.org/C_C++/socket.htm)


## Customizing Ubuntu 16

- open new instance of already open program e.g. Nautilus: middle click or shift+left click on icon in launcher


## Ubuntu keyboard shortcuts making our life easier

- find an extensive list [here](http://askubuntu.com/questions/28086/what-are-unitys-keyboard-and-mouse-shortcuts).

- alt + tab ... switch between open applications in the current workspace
- alt + ^ ... switch between open windows of the current application in the current workspace


# Unix in depth

## Partitions

- find information [here](https://en.wikipedia.org/wiki/Disk_partitioning) 
and [hier](https://de.wikipedia.org/wiki/Partition_(Datentr%C3%A4ger))


## Inodes

- inodes reside in a specially reserved block of the memory - which is usually 10% of memory capacity.
- inodes contain information about files and directories
    - ownership (group and userid)
    - access mode
    - file type
    - device id of the device containing the file
    - size of the file in bytes
    - ctime ... when was the inode last changed
    - mtime ... when was the file last changed
    - atime ... when was the file last accessed
    - link count (how many hard links point to the inode)
    - pointers to datacluster - can also be a pointer to a cluster further pointing to the actual data 
    (if the data is too extensive for one pointer in the inode)
- each inode is identified by an integer number

        // identify a files inode number
        ls -i [filename]

- one file can be linked multiple times. if a link points to an inode number, then the link is a `hard link` 
- if an inode is not referenced by a link, the corresponding data is removed from disk (???) when it is 
no longer used by any process. (or is it rather, that the pointers to the datacluster is removed & therefore free to be written to again?)
- A specific part of the filesystem, the `superblock` contains information about where the inodes themselves are found. 
- when a file is opened the following happens:
    - file system driver reads the superblock to get access to the inodes
    - the root of the inode directory is opened and the inode of the required directory where 
    the file resides in is looked up
    - the inode of the directory is used to look up the inode of the file
    - the inode of the file contains the link to the actual data of the file.

- find wikipedia information [here](https://en.wikipedia.org/wiki/Inode) and [hier](https://de.wikipedia.org/wiki/Inode)


# Move to fitting paragraph

## Redirecting to /dev/null

When redirecting to `/dev/null`, any command line output to `STDOUT` and `STDERR` from a program will be suppressed.

    e.g. the output from wget will be suppressed
    wget -O libgit2.tar.gz -o /dev/null https://github.com/libgit2/libgit2/archive/v0.24.5.tar.gz


## User management

Display all users

    getent passwd
or

    cat /etc/passwd 

or

    compgen -u

Add a new user

    # user friendly perl script using 'useradd' 
    sudo adduser [new_username]

or

    # native binary compiled with the system
    sudo useradd [new_username]

Delete user

    sudo userdel [username]

Remove home directory of a removed user

    sudo rm -r /home/[username]

Modify username

    usermod -l [new_username] [old_username]

Change Password

    sudo passwd [username]

Change shell of a user

    sudo chsh [username]

Change permissions of a group to the same as the user

    sudo chmod -R g=u


## Access System Error logs

Print all logs since the last restart:

    sudo journalctl -b -1


## Hardware information

Display CPU information

    lscpu
    # or
    cat /proc/cpuinfo

Display memory information

    lsmem
    # or
    cat /proc/meminfo

Display attached hardware

    lspci

Display attached usb devices

    lsusb

Display hostname (computer name within network)

    hostname
    # or
    hostnamectl

Display operating system details

    uname -a
    # or
    lsb_release -a
    # or
    hostnamectl

Display mac address

    ip link
    # or
    ifconfig -a
