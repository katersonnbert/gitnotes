# Subversion (SVN)

You can find the open source online subversion book [here](http://svnbook.red-bean.com).

#### Definitions:
- `Repository`  …   The directory storing the master copies of the project directories and files. 
                    The repository is a tree of directories.
- `Revision`    …   A numerical or alpha-numerical tag identifying the version of a file.

#### Organising project directories within the repository:
- `Trunk`   …   Main source code development folder, containing directory tree and working files of the project.
- `Tag`     …   Folder containing released version of the program. Once the program, that's being developed reaches 
                release state, copy contents of trunk into a new folder within Tag. Makes it easy to pull a released 
                version from the repository.
- `Branch   …   Folder containing version of the trunk, if two or more individual types of the project have to be 
                developed in parallel.

## Basic setup and workflow

Create repository: Most svn commands require a comment current svn step from the command line.

        -m "[text]"

Create repository folder at /path/ within folder dirName. Folder dirName must not exist.

        svnadmin create /path/repositoryDirName

Don't forget to set write rights for all login users

        chmod g+w -R /path/repositoryDirName

Actually create the repository. Imports directories and files from `path/[dirName]`, 
copies them into the repository `path/repDirName` and commits them as revision 1.
`-m ”[text]”` will be used as initial comment.

        svn import path/[dirName]/ file:///path/repositoryDirName -m "[text]"

Instead of "file:///path/repositoryDirName" you can also use:

        svn import path/[dirName]/ svn+ssh://server.address.com/path/repDirName -m "[text]"

Once you checkout a folder, the contents will be copied to your current directory. Furthermore, 
a hidden folder ”.svn” will be created at your current location, linking your working directory 
to the specified repository; every action within your working directory will always be executed 
towards this repository.

    svn checkout file:///repositoryPath/[dirName]

WATCH YOUR CURRENT DIRECTORY WHEN CHECKING OUT!
If you are e.g. at the following directory:

    /home/user/username/working_dir/proj1/trunk

and you try to check out directory trunk, this will happen:

    /home/user/username/working_dir/proj1/trunk/trunk

svn copies the complete folder trunk to your current location!

Commit transferes all changes made in the working directory to the repository.

    svn commit -m "[commit text]"

Commit [filename] transferes only changes of file [filename] to the repository.

    svn commit [filename] -m "[commit text]"

If you create or checkout your repository as svn+ssh you might want to generate an ssh client key at your main 
working machine(s) to avoid having to enter your authentication at every svn transaction:

    ~
    cd .ssh/
    ssh-keygen -t rsa
    cp id_rsa.pub authorized_keys


## Basic commands:

NOTE: always use commandline `option -m ”[description]”` with most of the following commands, 
where [description] should be a short text, explaining why/which changes have been made.

Update files in your working directory from the repository if new revisions are available. 
Files that have been modified at the repository as well as locally are merged automatically, 
if the changes are not overlapping. Should the differences overlap, a conflict will be displayed. 
Svn does not allow any commit, as long as a conflict is present.

    svn update

Available options to resolve update conflicts are:

    (p)ostpone      …   Leave the file in a conflicted state for you to resolve after your update is complete.
    (d)iff          …   Display the differences between the base revision and the conflicted file itself in unified diff format.
    (e)dit          …   Open the file in conflict with your favorite editor, as set in the environment variable EDITOR.
    (r)esolved      …   After editing a file, tell svn that you've resolved the conflicts in the file and that it should 
                        accept the current contents—basically that you've “resolved” the conflict.
    (m)ine-(f)ull   …   Discard the newly received changes from the repository and use only your local changes for the file under review.
    (t)heirs-(f)ull …   Discard your local changes to the file under review and use only the newly received changes from the repository.
    (l)aunch        …   Launch an external program to perform the conflict resolution. This requires a bit of preparation beforehand.

Update specified filename only within the working directory

    svn update [filename]

If a conflict has been identified by update or commit, svn needs the user to confirm, that the changes that have been 
made, can be safely transfered to the repository and overwrite the file of conflict there. Requires succeeding commit.

    svn resolve --accept [arg] [filename/dirname]

Available arguments for –accept are:

    mine-full   …   Resolve all conflicted files with copies of the files as they stood immediately before you ran svn update.
    theirs-full …   Resolve all conflicted files with copies of the files that were fetched from the server when you ran svn update.
    working     …   Assuming that you've manually handled the conflict resolution, choose the version of the file as it currently stands in your working copy.
    base        …   Choose the file that was the BASE revision before you updated your working copy. That is, the file that you checked out before you made your latest edits.

Add new files or directories to the repository. Changes will only become active after the next commit. 
Directories will be added including all contained files and subdirectories. To add a folder without files, 
use commandline option `–depth empty`.

    svn add [filename/dirname]

Marks a file or folder for deletion. Requires succeeding commit.
NOTE: nothing is ever really deleted from the repository, deleted files can be “resurrected”.

    svn delete [filename/dirname]

Duplicates [filename1] and adds it as [filename2]. Requires succeeding commit.

    svn copy [filename1] [filename2]

Move file from [filename1] to [filename2]. in detail [filename2] is added as copy of [filename1], 
[filename1] is marked for deletion. Requires succeeding commit.

    svn move [filename1] [filename2]

Same as mkdir [dirname]; svn add [dirname]. Requires succeeding commit.

    svn mkdir [dirname]

Display changes in file status and project structure:

    svn status

    ?   …   file is not under version control
    A   …   file is scheduled for addition
    C   …   file has conflicts with the repository version
    D   …   file is scheduled for deletion
    M   …   file has been modified locally, but not at the repository.
    U   …   file has been modified at the repository, but not locally
    G   …   file had non overlapping differences between repository and local file, files were merged.

Command line option -u (–show-updates) displays available updates

Display in-file differences between local and repository version

    svn diff  [filename/dirname]

Replace the local working copy of [filename] with the current revision copy from the repository.

    svn revert [filename]

Again, commit transferes all changes made in the working directory to the repository.

    svn commit -m "[commit text]"

Commit [filename] transferes only changes of file [filename] to the repository.

    svn commit [filename] -m "[commit text]"

## Further commands:

Display all revisions of the current repository; command line option -v: displays additional information

    svn log [-v]

Display all revisions of the current repository of [fileName] only

    svn log [fileName]
