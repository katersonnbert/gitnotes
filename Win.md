Windows good to knows
=====================

### Add to PATH variable:

- go to System Properties / Advanced
- Add the required folder to System variables: Path, terminate with a semicolon.


### Git under Windows:
- usually installed to `C:\users\[username]\AppData\Local\GitHub\PortableGit_[somehash]\`.


### Setup IDEA:
- disable star import - NOTE: do this for both imports and static imports!

        Settings > Editor > Code Style > Java > Imports > Class count to use import with '*'

- permanently show line numbers:

        Settings > Editor > General > Appearance > ... line numbers


### Setup Existing github repository with IDEA under Windows:
- Make sure, that git is in the windows PATH
- Make sure, that 
- Clone github repository locally
- Open IDEA, open project as maven -> tick import maven projects automatically -> finish


### Windows guest virtual box
- Windows guest in Linux host uses right-ctrl as host key
- Change view if the view seems screwed up - use combination until the view fits again.
- `[host key] + F`


## Useful CMD programs

- `where [command]` ... find installation directory of a program e.g. `where python`
- `find` ... find document containing a specific phrase.
    - will search all subdirectories for the phrase so might take very long depending on file structure
    - narrow down search by providing the files that are to be searched
    - provides globbing
    - case sensitive by default. use `find /i` to ignore case
