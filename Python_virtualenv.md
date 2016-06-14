Python virtual environment
==========================

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
 virtual environment. (not tested yet)

        virtualenv --system-site-packages [yourNewEnvironment]

- link the required libraries manually in the lib folder of the virtual environment.
NOTE: Make sure the versions of python you are using match!

        cd /home/[user]/pyvirtualenv/pymain/lib
        ln -s /usr/lib/python2.7/dist-packages/pygtk.py gtk.py
        ln -s /usr/lib/python2.7/dist-packages/gobject/
        ln -s /usr/lib/python2.7/dist-packages/glib/

- install the packages regardless in the active virtual environment (option not tested yet)

        /home/[user]/pyvirtualenv/pymain/bin/activate
        pip install --ignore-installed [package]

- For more details on this topic check
 [here](http://stackoverflow.com/questions/12830662/python-package-installed-globally-but-not-in-a-virtualenv-pygtk).
