HowTo install an Ubuntu VirtualBox under Windows
================================================

- DL VirtualBox from Oracle
- DL the latest Ubuntu Iso from Ubuntu
- Install VB to directory of choice
- Follow these [instructions](http://www.psychocats.net/ubuntu/virtualbox)

- Will install the Virtual box to `C:\Users\[username]\VirtualBox VMs\[Virtual box name]`

- Install VBGuestAddons to enable drivers for the guest OS
    - taken from [here](https://www.virtualbox.org/manual/ch04.html#mountingadditionsiso)

## Installing the Linux Guest Additions
The VirtualBox Guest Additions for Linux are provided on the same virtual CD-ROM file as the
Guest Additions for Windows described above.
They also come with an installation program guiding you through the setup process, although, due to the
significant differences between Linux distributions, installation may be slightly more complex.
1) Again, as with Linux hosts, we recommend using DKMS if it is available for the guest system.
If it is not installed, use this command for Ubuntu/Debian systems:

        sudo apt-get install dkms

Be sure to install DKMS before installing the Linux Guest Additions. If DKMS is not available or not installed,
the guest kernel modules will need to be recreated manually whenever the guest kernel is updated using the command

2) Mount VBoxGuestAdditions
- If you prefer to mount the additions manually, you can perform the following steps:
    - Start VBox
    - Settings -> Storage
    - add VBGuestAdditions.iso (usually under `C:\Program files\Oracle\VirtualBox`).

3) Start the VirtualBox
- change to the directory where your CD-ROM drive is mounted (usually `/media/[path]`)
 - execute as root:

        sudo sh ./VBoxLinuxAdditions.run

- For your convenience, we provide the following step-by-step instructions for freshly installed
copies of recent versions of the most popular Linux distributions.
- After these preparational steps, you can execute the VirtualBox Guest Additions installer as described above.

4) Restart the VirtualBox.
- Add shared folder between host and guest OS according to
[this](http://helpdeskgeek.com/virtualization/virtualbox-share-folder-host-guest/) and
[this](http://askubuntu.com/questions/30396/error-mounting-virtualbox-shared-folders-in-an-ubuntu-guest):

        sudo mkdir /mnt/guestDirName
        sudo chmod 777 /mnt/guestDirName
        sudo mount -t vboxsf hostDirName /mnt/guestDirName

- If there are network connection problems, check out the `/etc/network/interfaces` file.
    - [VB internet connection](http://www.mycodingpains.com/how-to-make-virtualbox-guest-use-its-hosts-internet-connection-and-still-have-ssh-access-to-the-guest/)
    - [Ubuntu guest VB](http://superuser.com/questions/789858/not-able-to-acces-internet-in-an-ubuntu-guest-in-a-windows-host-using-oracle-vir)
    - [Ubuntu 13 as host, Win7 as guest](http://askubuntu.com/questions/363003/no-internet-connection-on-virtualbox-windows-7-as-guest-ubuntu-13-04-as-host)

- Settings that worked for me:
    - first network connection: NAT without anything
    - second network connection: HostOnly Adapter


# Random notes

- [increase virtual box storage](https://www.maketecheasier.com/increase-virtual-hard-disk-size-in-virtualbox/)
- [shrink virtual box storage](https://www.maketecheasier.com/shrink-your-virtualbox-vm/)
