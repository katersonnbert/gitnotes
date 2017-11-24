Python virtual environment (1) and Anaconda (2)
===============================================

Virtual environments are nice due to various reasons I will add later when I don't want to do more productive things.

Since there are different versions of python and tons of different packages (yes, in different versions)
it helps to create environments where the python version and the installed packages are tightly controlled -> virtual envs.

## Install dependencies

### Install python package manager and virtualenv

        sudo apt-get install python-pip python-pip3

- if `python-pip3` fails, try installing `sudo apt-get install python3-pip`.

        sudo pip install --upgrade pip
        sudo pip install virtualenv

## Set up a virtualenv

### Prepare a folder for python virtualenv

        cd
        mkdir pyvirtualenv
        cd pyvirtualenv

- NOTE: be sure about the location of your virtualenv folder. It is possible to move a virtual env, but its no fun.

### Create and setup default python virtualenv

        virtualenv pymain

- the virtualenv command will create virtual environment and set it up with the default python version
of the operating system. Under Ubuntu 16 that's still python2.7.

- activate the virtual environment

        /home/[user]/pyvirtualenv/pymain/bin/activate

- install all python packages you want using pip. They will be installed within pymain and will only be
available, as long as the pymain virtual environment is active

- deactivate the virtual environment

        deactivate

# Handling global system libraries

- when creating a normal virtual environment, system libraries like glib or gobjects will not be included and
are not available within the virtual environment. Still when trying to install these using normal `pip` commands
within the virtual environment, the installer will complain.

- there are three solutions for this problem:

- when you want to use ALL globally installed packages in addition to your virtual environment,
 use the flag `--system-site-packages`. In this case all globally installed packages are available to the
 virtual environment. (recommended, works nicely)

        virtualenv --system-site-packages [yourNewEnvironment]

- link the required libraries manually in the lib folder of the virtual environment.
NOTE: Make sure the versions of python you are using match! (not recommended unless you really, annoying to try
and link until you get it right)

        cd /home/[user]/pyvirtualenv/pymain/lib
        ln -s /usr/lib/python2.7/dist-packages/pygtk.py gtk.py
        ln -s /usr/lib/python2.7/dist-packages/gobject/
        ln -s /usr/lib/python2.7/dist-packages/glib/

- install the packages regardless in the active virtual environment (option not tested so far)

        /home/[user]/pyvirtualenv/pymain/bin/activate
        pip install --ignore-installed [package]

- For more details on this topic check
 [Virtualenv user guide](https://virtualenv.pypa.io/en/stable/userguide/),
 [use site packages](http://stackoverflow.com/questions/12079607/make-virtualenv-inherit-specific-packages-from-your-global-site-packages),
 [link site packages](http://stackoverflow.com/questions/12830662/python-package-installed-globally-but-not-in-a-virtualenv-pygtk).


# Installed packages
Packages that have been installed via `pip` or `python setup.py xyz` can be found in 

    [path to virtualenv]/lib/[pyversion]/site-packages


# Troubleshooting

If `pip` is broken within a virtual environment, activate the virtual environment and run `easy_install pip`.
This should install the pip package again.


# PyCharm

- if you are using PyCharm as your python IDE, set the paths to the virtual environments here:

        File -> Settings -> Project -> Project Interpreter -> [Symbol] -> Add local -> Choose VirtualEnv path

    - for more information see
    [here](https://www.jetbrains.com/help/pycharm/2016.1/adding-existing-virtual-environment.html) and
    [here](http://stackoverflow.com/questions/10322424/how-to-select-python-version-in-pycharm).

- you can add different python interpreters to a project as described above.
- Once different interpreters have been added, run configurations can be defined to run / test the project
with different interpreters.

        Run -> Edit Configuration -> Change Python interpreter as you see fit -> save as "Run-[your config]"

===================================

# Anaconda

As python virtual environments, anaconda is an environment manager. But it not only provides python
specific environment handling, but also system packages.

The Anaconda documentation can be found [here](https://conda.io/docs). 

When not adding the conda bin directory to the operating systems' PATH variable, add convenient 
aliases for the command line e.g. und Linux aliases in .bash_aliases:

    # make conda command available
    alias conda='[path/to/conda]/bin/conda'

    # make activation of conda environments available; use as condact yourEnvName
    alias condact='source [path/to/conda]/bin/activate'

    # make deactivation of conda environments available
    alias condeact='source deactivate'

## Managing environments

### Create environments

Environments including their downloaded libraries and programs will be found at `[path/to/conda]/envs/`.

Create a conda environment with the default python version

    conda --create [envName]

Create a conda environment with a specific python version

    conda -- create [envName] python=3.5

Create a conda environment with additional packages

    conda --create [envName] [package]
