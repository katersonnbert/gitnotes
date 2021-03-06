Windows Powershell
==================

Get more infos out from [here](http://en.wikipedia.org/wiki/Windows_PowerShell).

## Background
The main application for the Windows PowerShell is automating tasks and providing a configuration management framework
for administrators. It consists of a command-line shell and an associated scripting language.
Administrators can write scripts that access both local and remote Windows systems and perform administrative tasks there.

Windows PowerShell is object oriented and is based on objects of the .NET framework. DOS commands have been
replaced by cmdlets ("Commandlets") which can be used in a pipe, connecting them with the "|" operator.

All datatypes mirror the .NET equivalents. PowerShell depends on the .NET-framework. [xxx]

## Getting help
PowerShell provides a command-line based help, similar to Unix man pages via the `Get-Help` cmdlet.
The content of the help can be updated by using the `Update-Help` cmdlet.


## PowerShell Scripts
Scripts executable by the PowerShell have to use the file ending `.ps1`. Functions, methods and definitions
that should be loaded when the PowerShell start have to be written to a file `profile.ps1` which has to be placed
in a specific folder, which location in turn depends on the operating system. [xxx]

## Cmdlets
The naming scheme of cmdlets follows "verb-substantive" e.g. `Get-Help`, `Get-Childitem`, `Get-Command`, `Set-Location`.

## PowerShell execution policies

The PowerShell supports a so called "execution policy" to provide administrators options to handle the
execution of shell scripts. The following four policies are supported:

- Restricted: no scripts can be executed, the shell operates as an interactive shell only;
NOTE: this is the default setting!
- AllSigned: runs scripts and config files, but only, if they are signed by a publisher that are explicitly
defined as trustworthy at least once.
- RemoteSigned: runs all scripts and config files. Scripts and config files downloaded from M Outlook,
Internet Explorer, Outlook Express, W Messenger must be signed by a trusted publisher, all other script sources
are not checked.
- Unrestricted: runs all scripts; all downloaded scripts can be executed after explicit confirmation but w/o restriction.

Can be set using the PowerShell prompt:

    Set-ExecutionPolicy AllSigned

## Signing a script
A script can be signed by a certificate authority (e.g. by Verisign) or by a user ("self signed certificate").

Self-signing scripts requires that the used certificate is installed on all computers that are running the
respective script. To create a self-signed certificate, the program `makecert.exe` (.NET Framework SDK) is used.
For a detailed description how to create a certificate and sign scripts with it,
 check [here](http://www.hanselman.com/blog/SigningPowerShellScripts.aspx).

## PowerShell IDEs:
Windows provides its own IDE, the PowerShell ISE (Integrated Scripting Environment) which is pre-installed on
Windows 7 and 8. The ISE provides basic managing of scripts: loading, saving, syntax highlighting, debugging.



# Basic PowerShell language:
## Comments
The hash character is required to use comments in PowerShell

## Variables:

All variables are preceded by a dollar sign:

    $var = "Yay!"

The type of the variable automatically adjusts to the contained value. In the example above,
variable `$var` has been assigned type string.
To ensure that a variable can only contain values of a specific data type, the type can be defined when a
variable is initialized:

    [int]$number = 102

Trying to assign a string to `$number` will now lead to an error.


## PowerShell data types

All data types from the .NET-framework are available:

    [int], [long], [double], [decimal], [float], [single], [byte], [string], [char]


## Initializing variables

By default PowerShell variables are created and initialized when they are first used.

Using command `set-psdebug-strict` forces the initialization of variables before they are used.


## Scope of variables

- global: variables are visible everywhere
- script: vars are visible everywhere within the script
- local: vars are only visible within the current block and in subblocks;
 NOTE: local is the default setting for variables [xxx]
- private: vars are only visible within the current block

explicitly define the scope of a variable:

    $global:var = "Yay!"


## Special variables

- `$args`... array containing all arguments passed to a function from the command line
- `$_` ... refers to the current object in the pipeline


## Arrays
PowerShell arrays start with index 0. Values are added by a comma separated list of values.
Adding values after initialization is done by using the `+=` operator. The size of an array can be accessed by
attribute "Length"

    $numVar = 1, 2, 3
    $strVar = "a", "b", "c"
    $strVar += "d", "e"
    $strVar.Length
    $getEl = $strVar[0]


## Operators
- PowerShell supports all arithmetic operators: `[+, -, *, /, %]`

        $var = $var + 1
        $var += 1           # equivalent to the line above.

- `[operator]=` can be used with all arithmetic operators
- concatenate strings:

        $var1 = "abc"
        $var2 = $a + "d"
        $var1 = "d"


## Comparators
PowerShell comparators return "True" or "False"

- `-eq`   equal
- `-ne`   not equal
- `-gt`   greater than
- `-ge`   greater or equal
- `-lt`   lower than
- `-le`   lower or equal


    5 -lt 10    # returns True
    5 -gt 10    # returns False


## if control structure
this one is easy as pie

    if (condition) {...}
    elseif (condition) {...}
    else (condition) {...}


## switch control structure
switch control structures execute all blocks where the condition is met. switch control structures can be used with
the additional parameters `-case` (case sensitive), `-wildcard` (usage of wildcard character)
and `-regex` (usage of regular expressions)

    switch (2) {
        1 {"one"}
        2 {"two"} }         # prints "two"

    switch -case ('abc') {
        'abc' {"lcase"}
        'ABC' {"ucase"} }   # prints "lcase"

    switch -wildcard ('abc') {
        'a*' {"a star"}
        '*c' {"star c"} }   # prints both "a star" and "star c"

    switch -regex ('abc') {
        '^a' {"starts with a"}
        'c$' {"ends with c"} } # prints both "starts with a" and "ends with c"


## Loop control structures
- while (condition) {...}

        $i = 0
        while ($i -lt 10) {
            echo $i
            $i++
        }

- do {...} while (condition)

        $i = 0
        do {
            echo $i
            $i++
        } while ($i -lt 1)

- for(initialize counter; condition; counter in/decrement) {...}

        for ($i = 0; $i -lt 10; $i++) {
            echo $i
        }

- foreach ($element in $list) {...}

        $arr = 1, 2, 3, 4
        foreach ($el in $arr) {
            echo $el
        }

- NOTE: `break` and `continue` can be used to stop a loop or immediately jump to the next loop cycle respectively.


## Accessing methods and object attributes
To access methods and object attributes, the point operator is used. PowerShell supports autocomplete by Tab.
If there are more than one method or attribute, multiple Tabs subsequently shows all the available completions.


## Text searching commands
Texts can be searched using wildcard search (`-Like`) or regular expression search (`-Match`). Both commands
return `True`, if a match has been found, `False` otherwise.

    $var1 = "abc"
    $var1 -Like "a*"        # Result True
    $var1 -Like "b*"        # Result False
    $var1 -Like "*b*"       # Result True
    $var2 = "adc"
    $var2 -Match "a[bd]c"   # Result True
    $var2 -Match "A[BD]C"   # Result True
    $var2 -Match "a[be]c"   # Result False
    $var2 -Match "a?c"      # Result True

NOTE: Both commands are case-insensitive.


# Sources and links:
Good tutorial with lots of different examples:
- http://www.computerperformance.co.uk/powershell

Extensive self study guide with lots of further links
- http://blogs.technet.com/b/musings_of_a_technical_tam/archive/2012/06/04/windows-powershell-self-training-guide.aspx

Official PowerShell documentation and scripting tutorial
- http://technet.microsoft.com/en-us/library/bb978526.aspx

PowerShell Module Reference
- http://technet.microsoft.com/library/hh847741%28v=wps.630%29.aspx

Windows PowerShell Integrated Scripting Environment (ISE), Pre-Installed under Windows 7 and 8:
- http://technet.microsoft.com/en-us/library/dd819514.aspx

Create and handle PowerShell certificates:
- http://www.hanselman.com/blog/SigningPowerShellScripts.aspx

Free PowerShell IDE IDERA "PowerShell Plus"
- https://www.idera.com/productssolutions/freetools/powershellplus

Video Introductions and Webcasts to Windows PowerShell
- http://technet.microsoft.com/en-us/scriptcenter/powershell.aspx
