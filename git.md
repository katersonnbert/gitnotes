git
===

# Managing git
- There are two main config files for git: .gitconfig for global settings in the /home/[user]/ directory 
and .git/config in each of the repositories for local repository specific settings.

- Check global git settings:

        less /home/[user]/.gitconfig

- Check git settings in main git folder:

        less .git/config

- Check a global setting entry e.g. user email

        git config --global user.email

- Set a global setting entry

        git config --global user.email "noone@nowhere.org"

- Check a local setting entry (from somewhere within a local repository)
 
        git config --get user.email

- Set a local setting entry (from somewhere within a local repository)

        git config user.email "noone@nowhere.org"

- To push to two different github accounts and commit users

    1) clone the repository that should use a different account using the https clone url
    2) update the email address in the local .git/config with the secondary account email address
 
For more information about this topic check [here](https://help.github.com/articles/setting-your-email-in-git/)


# Regarding commits
- always pull before a commit!
- commit always only affects the local repository
- when commit, use comment format: [filename/topic]: [short descr]
- when using vim:

        esc + i ... insert
        :x ... save and exit

# git commands
---|---
git status                  | shows project status
git add []                  | add files to git repository
git add .                   | adds all files recursively to repository which are not managed by git yet
git add -p                  | steps through all changes and asks if they should be staged for commit
git commit                  | commits all added or removed files to/from local git repository
git remote                  | show all remote repositories
git pull origin master      | pull from master branch
git pull origin [whatever]  | pull from [whatever] branch
git push origin master      | push changes from current branch to respective master branch on github
git push origin master      | if a project is forked, the local master is the fork, then upstream pushes  
    --set-upstream          |       to the original master from where the project was forked 
git branch                  | show local branches of project
git branch -a               | show all local and distant branches of project
git checkout [branchName]   | switch from current project branch to [branchName]
git merge [branchName]      | merge locally current project branch with [branchName]
git branch -D [branchName]  | delete branch [branchName]
git remote -v               | shows, which repositories are added
git log                     | displays commit history
git log --oneline --graph   | displays shortened commit history


## git add

You can do `git add -p yourFile` (or `--patch`), and git will begin breaking down your file in what it thinks are 
sensible "hunks" (portions of the file). You will then be prompted with this question:

Stage this hunk [y,n,q,a,d,/,j,J,g,s,e,?]?

And here the meaning of each option:

    y stage this hunk for the next commit
    n do not stage this hunk for the next commit
    q quit; do not stage this hunk or any of the remaining ones
    a stage this hunk and all later hunks in the file
    d do not stage this hunk or any of the later hunks in the file
    g select a hunk to go to
    / search for a hunk matching the given regex
    j leave this hunk undecided, see next undecided hunk
    J leave this hunk undecided, see next hunk
    k leave this hunk undecided, see previous undecided hunk
    K leave this hunk undecided, see previous hunk
    s split the current hunk into smaller hunks
    e manually edit the current hunk
    ? print help

If the file is not in the repository yet, do first `git add -N yourFile`. 
Afterwards you can go on with `git add -p yourFile`.

You can then use:

- `git diff --staged` afterwards to check that you staged the correct ones
- `git reset -p to unstage` incorrect hunks
- `git commit -v` to view your commit while you edit the commit message.

Paraphrase from [here](http://stackoverflow.com/a/1085191)


# Fork/clone a project from github

Forking a project from another user means creating a copy of this project, that is detached from the original.
Work (including pushing commits to) the forked repository will now not directly affect the original repository.

NOTE:
- never use the `https://github.com/...` address for forks
- use `git@github.com:...` instead!
- If that somehow happens, the paths can be replaced in `.../[mainGitDir/.git/config]`

- fork project in github to your user
- copy ssh clone URL e.g. `git@github.com:mpsonntag/gndata-editor.git`
- terminal: move to folder you want to create your repository
- clone repository

        git clone [git ssh clone URL] [projectFolderName]
        e.g. ~/work$ git clone git@github.com:mpsonntag/gndata-editor.git GNdata-editor

- set upstream repository!

        git remote add upstream [git ssh spoon-knife clone url]

#### How to specify the original repository to our fork (the "spoon-knife" repository)
- get clone url of spoon-knife repository from github
- move to the local git repository dir
- check which repositories are already used

        git remote -v

- add upstream repository by using

        git remote add upstream [git ssh spoon-knife clone url]

- now we have specified the upstream repository for our fork repository


#### Change the url of a remote
 
    git remote set-url origin [different url]


#### How to sync fork repository with upstream repository
- to get modifications done in the upstream repository

        git fetch upstream

- this creates a local branch of the upstream repository

        git remote -v
        
- next make sure you are in the master fork repository

        git checkout master

- then merge the upstream repository with the fork repository

        git merge upstream/master


# Pull request
If everyone is working on their own forks, then a pull request lets other people know, that there are major changes 
in your fork and they should sync their forks with the contents of your fork.

#### Creating a pull request
In github under your user and your fork, look on the left hand side above the file list. there is a green button 
and a branch button. 
select the branch you want to compare and then use the green button, to see, if there are any changes between the 
spoon and the fork repositories of the selected branch. 
Through the following menu you can create a pull request for all other users.

#### Handling a pull requires
If there is an open pull request, it can be seen on the right hand side


# Branches
Branches are useful for working on stuff, while always keeping a master branch that contains the last working version. 
Once the branch version is functional, the changes can be merged with the last functional version of the master branch.

- first create a new branch and immediately switch to this branch

        git checkout -b [name of new branch]

- next we push this branch to github

        git push origin [name of new branch]

- we can check all existing branches

        git branch

- show differences between branches

        git diff [branch1]..[branch2]

#### Merging branches
- checkout branch you want to import changes into

        git checkout [branch1]      // ... e.g.:  git checkout master

- then merge

        git merge [branch2]         // ... e.g.: git merge workInProgress


# Push
- normal push moves always all associated branches to remote

        git push

- to push only a specific branch to the remote repository

        git push origin [branch name]


# Creating a github from local repository
- create a github repository that's named exactly as the folder you want to add on github
- move to folder containing files
- initialize git

        git init

- create file .gitignore and modify

        *_exc_*
        *~

- locally add all files to repository (will not add empty folders)

        git add .

- commit

        git commit -m 'hurra'

- add remote repository

        git remote add origin [repo ssh origin url]
        git remote -v

- push stuff to github

        git push origin master


# git rebase
A very good explanation can be found [here](http://git-scm.com/book/de/v1/Git-Branching-Rebasing)


# G-Node git pipeline
- go to github
- go to your project
- open Milestone of interest
- go to issues (right hand side)
- create a new one, assign milestone and person and submit

        git fetch --all
        git rebase upstream/master

- In IDEA go to tools
- go to tasks & context, select configure server
- enter host github.com

        repository G-Node / [repository name] e.g. gndata-editor

- either enter API token that already exists for a project or create new one
- on right hand side upper corner select default task/add task ... issues from github should be imported.
- if this is correct, IDEA creates a new branch for this issue
- make changes in project, add changed files in "changes"
- use "commit", write one line description, leave one empty line, write "Fixes #issuenumber" 
if this commit actually fixes the number

        git log                         // ... check last commits
        git push origin [branchname]    // ... push issue branch to repository e.g.: git push origin gndata-editor-46

- someone has to check the changes on github and merge the branch with the master

        git status

- change to master

        git fetch --all
        git rebase upstream/master          // ... to get the changes included into the own master repository
        git log
        git push origin master
        git branch -D gndata-editor-46      // ... remove issue branch locally
        git push origin :gndata-editor-46   // ... remove issue branch in repository


## Creating pull request
- github: create pullrequest (process self explanatory)
- if pull request cannot be merged

        git checkout master
        git fetch --all
        git rebase upstream/master
        git checkout [problem branch]
        git rebase master
        git status                          // ... check where the conflicts are, resolve them in IDE, save
        git add [modified files]            // ... will be added to last commit
        git status
        git rebase --continue
        git log
        git push origin [problem branch]    // ... if this does not work, use force:
        git push origin [problem branch] -f

        git commit --amend                  // add changes to last commit,
                                            //      opens editor for changes in the commit message
                                            // Be aware, that every "amend" will change the commit hash.
                                            // If the edited commit has been pushed to a remote, a new push will require force,
                                            // since it needs to replace the existing commit remotely.
        git commit --amend -m "New message" // add changes to last commit, use new commit message
        git commit --amend --no-edit        // add changes to last commit, do not change commit message (git v1.7.9+)


## Creating an intermediate branch and subsequent pull request with these changes only
- start

        git checkout -b [newbranch]     // ... create new branch

- do stuff

        git add .                       // ... add changes
        git commit                      // ... commit changes
        git push origin [newbranch]     // ... push changes to github

- create pull request in github 
- pull request
- edit
- select branch to pull
- add description
- create pull request

        git checkout [workBranch]   // get back to actual workBranch
        git rebase [newBranch]      // get changes from [newBranch] to [workBranch]


# Submodules (git within git)

Move to folder where the submodule is supposed to be installed

        git submodule add git@github.com:mpsonntag/testBoot.git stylesheets

Detailed description [here](http://git-scm.com/book/en/v2/Git-Tools-Submodules).


# Create github repository from local

From [here](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/#platform-linux).

- initialize local git repository

        git init

- create .gitignore file, open and add (or copy from another project):

        # IDEA
        .idea/
        *.iml

        # log files
        *.log

        # maven
        target/
        pom.xml.tag
        pom.xml.releaseBackup
        pom.xml.versionsBackup
        pom.xml.next
        release.properties

- add all files to git and commit

        git add .
        git commit -m "first commit"

- create new repository in github using the same name as the folder the project resides in; do not initialize the github repo
- copy the quick setup ssh address of the github repository e.g. git@github.com:mpsonntag/G-Node-Bootstrap.git
- add the github repository as the remote repository for the local git, e.g.:

        git remote add origin git@github.com:mpsonntag/G-Node-Bootstrap.git

- push changes from local to remote

        git push origin master

- have a cup of tea

# Discard changes from unstaged files

        git checkout [file_name]

Read details [here](http://stackoverflow.com/questions/22620393/various-ways-to-remove-local-git-changes)

# Undo / remove last commit:
You want to NUKE THE LAST COMMIT AND NEVER SEE IT AGAIN. You do this:

        git reset --hard HEAD~1

WARNING: this will also remove ALL UNTRACKED CHANGES!

Read details [here](http://stackoverflow.com/questions/927358/how-to-undo-the-last-commit)


# Find difference between commits:

Source can be found [here](http://jk.gs/git-diff.html)

- show the difference between the last two commits:

        git diff HEAD^ HEAD


# git delete
- delete local branch

        git branch -d the_local_branch

- delete remote branch

        git push origin :the_remote_branch


# git amend commit (add stuff, change message)
- add stuff to the last commit

        git commit --amend

- add stuff to the last commit w/o changing the commit message

        git commit --amend --no-edit

- change commit message of last commit

        git commit --amend -m "New commit message"


# Amending more than one commit: interactive rebase

Distilled from 
[Atlassian](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase-i) and 
[here](https://robots.thoughtbot.com/git-interactive-rebase-squash-amend-rewriting-history).

- If commits should be changed that lie somewhere in the past, git offers interactive rebasing.
- The commit has to be specified, from which the rebase should be started

        git rebase -i <specify commit from where to start rebasing to the current commit>

- rebase all commits of the current branch compared to a different branch:

        git rebase -i <branch>
        e.g.
        git rebase -i master

- rebase all commits starting from a specific commit in the current history:

        git rebase -i HEAD~4


- When the starting point of the interactive rebase has been chosen, the terminal will list all commits since then, 
    it can be interactively chosen, which commits and how the selected commits then should be amended:

        e.g.
        pick ee056f1 Postgres: add export table data
        pick f1f62a0 Postgres: add import table data
        pick 40b0e70 css: add reference link
        
        # Rebase db073f7..f7236c7 onto db073f7 (3 command(s))
        #
        # Commands:
        # p, pick = use commit
        # r, reword = use commit, but edit the commit message
        # e, edit = use commit, but stop for amending
        # s, squash = use commit, but meld into previous commit
        # f, fixup = like "squash", but discard this commit's log message
        # x, exec = run command (the rest of the line) using shell
        # d, drop = remove commit
        #
        # These lines can be re-ordered; they are executed from top to bottom.
        #
        # If you remove a line here THAT COMMIT WILL BE LOST.
        #
        # However, if you remove everything, the rebase will be aborted.
        #
        # Note that empty commits are commented out


# Cherry pick a commit

- when doing e.g. a hotfix, that is supposed to be applied to multiple branches, 
the commit containing these changes can be copied - "cherry picked" - from one branch 
to any other branch.
- find out the hash of the required commit e.g. `145cef6`
- checkout the branch where the commit is supposed to picked into
- use the following command

        git cherry-pick [commit hash]
        e.g.
        git cherry-pick 145cef6

- git log should now show the commit applied to the top of the commit stash.


# Transfer ownership from user to organization:

Distilled from [here](https://help.github.com/articles/transferring-a-repository/)

- update github repository
- close IDE
- rename local project folder as backup
- transfer ownership
- fork from organisation to user
- move to main directory, clone user repository
- set upstream
- copy .iml file and .idea folder from backup folder to new folder
- open IDE, check if everything works
- delete backup folder

- if there were CI links, these have to be updated as well. (G-Node appvoyer repo runs via adrians user)


# Create branch from previous commit:

e.g. to create a save branch when a specific release has been done.

        git branch branchname <sha1-commit-hash>

Paraphrase from [here](http://stackoverflow.com/questions/2816715/branch-from-a-previous-commit-using-git)


# Manage branches

- Show all local branches

        git branch

- Show all branches, local and remote

        git branch -a

- Show all remote branches

        git branch -r

Distilled from [here](http://gitready.com/intermediate/2009/02/13/list-remote-branches.html)


# Remove stale upstream repositories

If working on with distributed repositories, it can happen, that stale remote repositories 
can be shown when using git branch -a or git checkout

Remove stale remote repositories by:

        git remote prune [origin/upstream]

You can display which repositories will be removed by using first:

        git remote prune --dry-run [origin/upstream]


# Unstage file for commit

            git reset <file>


# Commit messages
Adhere to [these rules](http://chris.beams.io/posts/git-commit/)

- Separate subject from body with a blank line
- Limit the subject line to 50 characters
- Capitalize the subject line
- Do not end the subject line with a period
- Use the imperative mood in the subject line
- Wrap the body at 72 characters
- Use the body to explain what and why vs. how


Add a commit message from the command line:

        git commit -m "Message"

Write multi line commit messages from the command line - ' or " can be used to write multi line commit messages:

        git commit -m "Message head
        BodyText
        More Body Text

        Final line"

This might be tricky on windows. For such issues [read this]
(http://stackoverflow.com/questions/5064563/add-line-break-to-git-commit-m-from-command-line)


# Automatically close issues using commit messages:
Issues can be automatically closed by using keywords in the text body of COMMIT MESSAGES (NOT pull requests!)

These keywords are:

        close #[issueNr]
        closes #[issueNr]
        closed #[issueNr]
        fix #[issueNr]
        fixes #[issueNr]
        fixed #[issueNr]
        resolve #[issueNr]
        resolves #[issueNr]
        resolved #[issueNr]

Read more details [here](https://help.github.com/articles/closing-issues-via-commit-messages/)


# Move commits from master branch to new branch.

- checkout new branch

        git checkout -b "newBranch"

- new branch and master branch are now identical
- checkout master
- remove the last commits that should only be in the new branch.
- if it is only one commit:

        git reset --hard HEAD~1

- if it is n commits:

        git reset --hard HEAD~n


# Bisect - fast find breaking commit in history

The actual commands you need to run to execute the full git bisect flow are:

- Let git know to start bisecting.

        git bisect start
- Let git know about a known good commit (i.e. last commit that you made before the vacation).

        git bisect good {{some-commit-hash}}
- Let git know about a known bad commit (i.e. the HEAD of the master branch). git bisect bad HEAD (HEAD just means the last commit).

        git bisect bad {{some-commit-hash}}
- At this point git would check out a middle commit, and let you know to run your tests.
- Let git know that the feature does not work in currently checked out commit.

        git bisect bad
- Let git know that the feature does work in currently checked out commit.

        git bisect good
- When the first bad commit is found, git would let you know. At this point git bisect is done.
- Returns you to the initial starting point of git bisect process, (i.e. the HEAD of the master branch).

        git bisect reset
- Log the last git bisect that completed successfully.

        git bisect log


## stash changes

If there are changes that should not be staged for a commit, they can be stashed. This way branches can be switched 
without committing any unfinished changes.

        // stashes all currently untracked changes
        git stash

        // re-apply stashed changes
        git stash apply

Find more details about stashing and using different stashes [here](https://git-scm.com/book/en/v1/Git-Tools-Stashing).


## Signing commits and tags

- [git signing](https://git-scm.com/book/tr/v2/Git-Tools-Signing-Your-Work)
- [extensive examples](https://mikegerwitz.com/papers/git-horror-story)


## Complementary software tools

###  tig

[tig](https://github.com/jonas/tig) makes browsing the history, viewing diffs and cherry picking really convenient.
