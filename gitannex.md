git annex
=========

Follow this [tutorial](https://git-annex.branchable.com/walkthrough/)

- when using usb devices, make sure that you do not auto mount. otherwise
    - open `gnome-disks`, check the file system path of the device, e.g. `/dev/sdg1` or `/dev/sdf1`
    - stop the corresponding device via `disks`.
    - mount the device on the command line (make sure the target folder e.g. `/mnt/usb` exists)

            sudo mount /dev/sdg1 /mnt/usb

- if this is not observed, the above mentioned tutorial will fail.
