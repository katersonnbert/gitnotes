Concurrent Versions System (CVS)
================================

## Terminology:

- 'Repository`  ... The directory storing the master copies of the files.
    - The main or master repository is a tree of directories.
    - Rule #1 of keeping CVS happy : DO NOT EDIT FILES IN THE REPOSITORY!
- `Module`      ... A specific directory (or mini-tree of directories) in the main repository.
                        Modules are defined in the CVS modules file. 
- RCS           ... Revision Control System. A lower-level set of utilities on which CVS is layered.
- `Revision`    ... A numerical or alpha-numerical tag identifying the version of a file.
- `Tag`         ... A certain milestone in a file or module's development.
- `Branch`      ... A 'fork' of the module


## Basic command set:

    cvs checkout [name]     ... creates a copy of the module [name] from the repository in your current folder.
                                additionally, checks out the module in the repository.
    cvs co [name]           ... s.a.

    cvs update              ... updates your local files with those from the repository
                                prefix U ... update at the server
    cvs login               ... does exactly what it sounds like

important files in folder CVSROOT
    checkoutlist


## Restricting access:

- place file "writers" in folder CVSROOT containing the usernames of user that have writing rights.
- To edit this file use command:

        cvs add writers
        cvs edit checkoutlist
        cat >> checkoutlist
        writers
        ^D
        cvs commit -m


## Link list
- http://www.linux.ie/articles/tutorials/cvs.php
- http://vasc.ri.cmu.edu/old_help/Archiving/Cvs/cvs_tutorial.texinfo_toc.html
