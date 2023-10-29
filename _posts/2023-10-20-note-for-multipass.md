# Note for Multipass

## Install for Mac
Install multipass.
```bash
brew install --cask multipass
```

Install for GUI.
```bash
brew install --cask microsoft-remote-desktop
```

## Usage
Create a VM. `impish` is the version of Ubuntu. Use `multipass find` to get a version list.
```bash
multipass launch --name vm1 --memory 2G --disk 40G --cpus 2 impish
```

Get a configuration of a VM.
```bash
sudo cat /var/root/Library/Application\ Support/multipassd/qemu/multipassd-vm-instances.json
```

Change a configuration file.
```bash
# stop multipassd 
sudo launchctl unload /Library/LaunchDaemons/com.canonical.multipassd.plist

# edit /var/root/Library/Application\ Support/multipassd/qemu/multipassd-vm-instances.json
# you'll need sudo for that
sudo vi /var/root/Library/Application\ Support/multipassd/qemu/multipassd-vm-instances.json

# start multipassd again
sudo launchctl load /Library/LaunchDaemons/com.canonical.multipassd.plist
```

Change password.
```
sudo passwd ubuntu
```

Config proxy.
```bash
# set proxy
export ALL_PROXY=socks5://{IP}:{PORT}

# test proxy
curl -vv https://www.google.com

# Unset proxy.
unset ALL_PROXY
```


## Usage of GUI
Install GUI support.
```bash
sudo apt update
sudo apt install ubuntu-desktop xrdp
```

Get IP address.
```bash
ip addr
```

## Troubleshoot

### Modify a disk size if the disk is full
Following is the instruction of extending disk spaces.
```bash
# stop and set the size of the disk.
# generally multipass will have all the things done.
# but at the case of full disk, it just give the size but not adjust the OS.
multipass stop {instance}
multipass set local.{instance}.disk={size}G
multipass start {instance}
multipass shell {instance}
```

After login the shell, run `sudo lsblk` to see the disk info.
```bash
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0     7:0    0 91.9M  1 loop /snap/lxd/24065
loop1     7:1    0 35.5M  1 loop /snap/snapd/20298
loop2     7:2    0 59.3M  1 loop /snap/core20/2019
sda       8:0    0   10G  0 disk
├─sda1    8:1    0  4.9G  0 part /
└─sda15   8:15   0   99M  0 part /boot/efi
vda     252:0    0   52K  1 disk
```

Then run `sudo parted`, follow the commands below.
```bash
GNU Parted 3.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) resizepart
Warning: Not all of the space available to /dev/sda appears to be used, you can fix the GPT to use all of the space (an extra 10485760 blocks) or continue with the current setting?
Fix/Ignore? Fix
Partition number? 1
Warning: Partition /dev/sda1 is being used. Are you sure you want to continue?
Yes/No? Yes
End?  [5369MB]? 100%
(parted) quit
Information: You may need to update /etc/fstab.
```

Run the following commands for extending disk space to file system.
```
sudo resize2fs /dev/sda1
resize2fs 1.45.5 (07-Jan-2020)
Filesystem at /dev/sda1 is mounted on /; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 2
The filesystem on /dev/sda1 is now 2595579 (4k) blocks long.
```

Run `df` to check the disk space available.

## References

> from https://blog.csdn.net/qq_41437512/article/details/125033563

> from https://multipass.run/docs
