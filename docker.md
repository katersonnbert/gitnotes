
# docker installation

## Ubuntu linux

Install docker using `apt`:

    sudo apt install docker.io

With this docker can only be run as root user. Add your user to the docker usergroup to be able to run it as well.

    sudo usermod -a -G docker $USER

This will append (`-a`) `$USER` to the usergroup (`-G`) `docker`. The current usergroups can be found and checked in `/etc/group`.
It will need a re-login for the changes to take effect.

https://www.linux.com/learn/intro-to-linux/2017/11/how-install-and-use-docker-linux

