Linux bash (bourne again shell)
===============================

#  Using the shell

Baby steps:
1) get someone to show you how to open a shell and execute commands
2) get to know the file system and how to navigate through it (see explanatory tutorial above)
3) get to know the basic shell commands described below
4) script away


##  More resources:

Understanding the unix file system:
http://www.december.com/unix/tutor/filesystem.html

and
http://linuxcommand.org/lts0040.php

Shell Tutorial
http://linuxcommand.org/learning_the_shell.php


# Navigating the filesystem:
When you first open your shell you will see something like this:
    
    [chris@troll ~]$

This means, that you have logged into a linux network with username "chris", that you currently use the computer named "troll" and are currently within the space, the filesystem has reserved as your workspace (indicated by "~").

In a more abstract way will always see the information like this:

	[username@computername current_folder]$

You can navigate through the filesystem by using the "cd" (change directory) command right after the "$" sign. Work yourself through the navigation introduction provided with the links above.


# Linux nice to knows:

- Special characters which are not allowed in file- or folder names:

    ` /, .., ~, *, ?, >, <, |, \`

- Linux is case sensitive!
	`Filename.txt` is not the same as `filename.txt`

- Try to avoid blanks within file- or folder names, always use underscore or minus, especially, if you create folders or files using the filebrowser!
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

`ls`        ... display files and directories within your current directory
`ls -l`     ... like ls, but display additional information
`ls -a`     ... like ls, but also display hidden files and directories
`ls -l -a`  ... combination of `ls -l` and `ls -a`

ls ../Foldername ... display the contents of directory "Foldername" residing on the same hirarchical layer as the folder we are currently residing in

ll ... like ls -l

pwd ... print working directory, prints the complete path from root until the directory we are currently residing in

        e.g. [chris@troll papers]$pwd
             /home/users/chris/papers/
             [chris@troll papers]$

cd [path] ... change directory; move within the filesystem from your current directory to another directory specified in your [path]

        e.g. [chris@troll ~]$cd /home/user/franz/

cd ..   ... change from the current directory to the directory directly above.

mkdir foldername ... create folder "foldername" at your current position within the filesystem

        e.g. [chris@troll papers]$mkdir cell_papers

rmdir foldername ... remove folder "foldername" from the filesystem. will only work, if the folder is empty.

        e.g. [chris@troll papers]$rmdir cell_papers

rm filename ... remove file "filename" from the filesystem.

        e.g. [chris@troll papers]$rm 2011_Science_2282772.pdf

rm foldername -r ... remove folder "foldername" from the filesystem including all files and folders it contains.

        e.g. [chris@troll work]$rm papers -r

cp path1/source path2/target ... copy file "source" residing at location "path1" to file "target" residing at location "path2"

        e.g. [chris@troll work]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf /home/users/chris/work/papers/science/2011_Science_2282772.pdf

cp path1/source ./target ... copy file "source" residing at location "path1" to file "target" at the current location

        e.g. [chris@troll science]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf ./2011_Science_2282772.pdf

will copy the file 2011_Science_2282772.pdf from location /home/user/chris/work/papers/ to location /home/users/chris/work/papers/science/

cp path1/source . ... copy file "source" residing at location "path1" to the current location, keeping the same filename.

        e.g. [chris@troll science]$cp /home/user/chris/work/papers/2011_Science_2282772.pdf .
will copy the file 2011_Science_2282772.pdf from location /home/user/chris/work/papers/ to location /home/users/chris/work/papers/science/

mv [Pfad-Quelle]/[Filename] [Pfad-Ziel] ... same as "cp" command, but moves the file from one location to the other, deleting the original file.


echo text ... prints "text" onto the screen

        e.g. [chris@troll work]$echo hurray for icecream!
        
echo text > filename.txt ... saves "text" into file "filename.txt" which will be created at the current filesystem location

        e.g. [chris@troll work]$echo hurray for icecream! > important.txt

cat filename ... prints the contents of file "filename" onto the screen.

        e.g. [chris@troll work]$cat important.txt

less filename ... displays the contents of file "filename" in the screen, contents are scrollable by using "up" and "down" keys. end this by pressing "q"

        e.g. [chris@troll work]$less important.txt

history ... list of all commands executed within this terminal

        e.g. [chris@troll ~]$history

exit ... close the shell

        e.g. [chris@troll ~]$exit



# Further Basic Bash command line options:

[command] > [filename] ... will write the output of a specifc [command] to a specific file. note, that an already existing file with the same filename at the same location will be replaced!

        e.g.    [chris@troll work]$echo hurray for icecream! > important.txt
                [chris@troll work]$less important.txt
                [chris@troll work]$echo another hurray for schnitzel! > important.txt
                [chris@troll work]$less important.txt

[command] >> [filename] ... will append the output of a specific [command] to a specific file. If the file does not exist yet, it will be created.

        e.g.    [chris@troll work]$echo chicken >> shopping_list.txt
                [chris@troll work]$less shopping_list.txt
                [chris@troll work]$echo onions >> shopping_list.txt
                [chris@troll work]$echo lemons >> shopping_list.txt
                [chris@troll work]$echo olive oil >> shopping_list.txt
                [chris@troll work]$echo rosemary >> shopping_list.txt
                [chris@troll work]$less shopping_list.txt

[command] & ... start a program running in the background, getting back the shell.

        e.g.    [chris@troll work]$firefox &

[command]       ... start a program

        e.g.    [chris@troll work]$firefox

ctr + z         ... stop the execution of the program and keep it on hold in the background
bg              ... restart the program, but keep it running in the background (bg)

                [chris@troll work]$bg

fg              ... if you have a program running in the background, get it back by fg (foreground)

        e.g.    [chris@troll work]$fg

command_1 | command_2 ... | ... "pipe". executes "command_1", directs the output of this command not to the screen, but the second command "command_2". Only then the output of "command_2" will be printed onto the screen.



# Further useful Bash commands:

`ssh computername` ... connect to another computer using secure shell (an encrypted connection to another computer within the network). once connected, the cpu of this computer will be used for all further actions. makes sense to do large calculations on the server rather than on the local machine. Might require a password.

        [chris@troll work]$ssh server4
        [chris@server4 ~]$

Notice, that the computername in the bash shell has changed from "troll" to "server4"

Use the command "exit" to close the ssh connection to another computer.

        [chris@server4 ~]$exit
        [chris@troll ~]$

`scp computername:remote_directory/filename current_directory`   ... copy file "filename" from a remote computer or network into the "current_directory" using encrypted file copy

        e.g. [chris@troll work]scp server4:/temp/work/* /home/user/chris/work/
        	
`wc filename` ... counts lines, words and letters within file "filename"

        e.g. [chris@troll work]wc shopping_list.txt


# More nice bash commands, use the --help option for in depth information:

top     ... displays all processes running on the currently used computer. exit using "q"

gzip    ... compress/uncompress files

tar     ... compress/uncompress files


# Commandline programs for textfile handling:
Use the --help option for in depth information, links provided or the interweb.

sort    ... Sorts standard input then outputs the sorted result on standard output.
uniq    ... Given a sorted stream of data from standard input, it removes duplicate lines of data (i.e., it makes sure that every line is unique).
fmt     ...Reads text from standard input, then outputs formatted text on standard output.
pr      ...Takes text input from standard input and splits the data into pages with page breaks, headers and footers in preparation for printing.
head    ... Outputs the first few lines of its input. Useful for getting the header of a file.
tail    ... Outputs the last few lines of its input. Useful for things like getting the most recent entries from a log file.
tr      ... Translates characters. Can be used to perform tasks such as upper/lowercase conversions or changing line termination characters from one type to another (for example, converting DOS text files into Unix style text files).
grep    ... Examines each line of data it receives from standard input and outputs every line that contains a specified pattern of characters.
                http://www.panix.com/~elflord/unix/grep.html

sed     ... Stream editor. Can perform more sophisticated text translations than tr.
                http://www.ceri.memphis.edu/computer/docs/unix/sed.htm
awk     ... An entire programming language designed for constructing filters. Extremely powerful.
                http://www.vectorsite.net/tsawk.html



# Root access by using `sudo`
[Introduction to sudo](https://www.linux.com/learn/tutorials/306766:linux-101-introduction-to-sudo)

In a nutshell sudo is the commandline way to grant a user root access for the following command e.g. when installing a software package. It will require a password that is associated with a user that has root access.

	sudo [command]
	[sudo] password for [username]:


# Installing packages by using apt (Advanced Packaging Tool)
[Quick apt-get overview](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool)

When working on Debian based Linux systems, apt-get is a convenient way to download and install software packages.
For most of the packages root access is required which means using `sudo apt-get [command]`.

- Update the index to be up to date with all the packages that can be installed via apt-get
	sudo apt-get update

- Install a package e.g. the terminator linux terminal, which is an awesome linux terminal
	sudo apt-get install terminator

- Install a package that is not handled via the normal apt-get package list e.g. the java8 distribution: This requires to manually add a repository to the apt-get list of installable packages, update the repository and the install the package.
	sudo add-apt-repository ppa:webupd8team/java
	sudo apt-get update
	sudo apt-get install oracle-java8-installer


# Using SFTP with the file browser
- ctrl + l
- enter user@url e.g. nix@projects.g-node.org
- enter pass phrase
 

# Finding where a program is installed

    which [program name]



# File operations

### Create file

    touch fileName

### Overwrite file

    echo "text" > fileName

### Add to file at the bottom

    echo "text" >> fileName


## File permissions

### Changing permissions:
    
    chmod options permissions filename

### Change permissions to read, write and delete for everyone:

    chmod 777 filename

Detailed description and examples:
http://www.computerhope.com/unix/uchmod.htm


## SymLinks (Symbolic links) to files or folders

### Create Symlink (symbolic link)
http://www.cyberciti.biz/faq/creating-soft-link-or-symbolic-link/

        ln -s [target-filename] [symbolic-filename]

This creates a symbolic link named [symbolic-filename] that points to [target-filename]
NOTE: ALWAYS use the whole path as [target-filename], do not use relative terms.

        e.g. ln -s /home/currentUser/tmp/testLinks/hurra.txt ../hurraLink

### Delete Symlink
Symlinks can simply be deleted by rm. This will not touch the file the link points to.

        e.g. rm hurraLink



# Important system folders in Linux:

###  Global searchpath variable $PATH:

- display all directories currently included in the searchpath:
	    `echo $PATH`

- permanently add directories to `$PATH` by editing `/etc/profile` or `$HOME/.profile` by adding
	    `export PATH=$PATH:/[directory of choice]`

- Symlinks for the operating system:
    	`/etc/alternatives`

- Custom packages and libraries can usually be found in one of the following folders:

        /opt/
        /usr/local/include/
        /usr/local/lib/ & /usr/local/lib/pkgconfig/
        /usr/share/
        /home/[user]/bin/


# Find text in files using grep

- Find text "NoResultsException" in all files of all subfolders (-R) of the current folder (.) and print the line number where the match has been found (-n).
	`grep -Rn "NoResultsException" .`


# netstat: check connections and available sockets

	`netstat`
	
- Check connections:
	`netstat -l`
- Check connections showing their local address
	`netstat -lt`


# Linux server crashcourse:

http://www.linuxhowtos.org/C_C++/socket.htm

- client and server are both processes, that communicate with each other via SOCKETS.
- processes can only communicate, if they are in the same ADRESS DOMAIN and have the same SOCKET TYPE.
- the two main different address domains are the UNIX DOMAIN (processes live in a common file system) and the INTERNET DOMAIN (processes live on two hosts anywhere in the internet)
- the address of a socket in the unix domain is a CHARACTER STRING entry in the file system
- the address of a socket in the internet domain is the internet adress of the host machine which is the IP ADDRESS (unique 32bit address).
- Each internet socket address needs a port number on its host (16bit unsigned integer).
- There are port numbers reserved for Unix standard services, port numbers above 2000 are available.
- The mainly used socket type is the STREAM SOCKET. Stream sockets TCP (transmission control protocol).



