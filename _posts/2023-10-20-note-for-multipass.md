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
multipass launch --name vm1 --mem 2G --disk 40G --cpus 2 impish
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
```
ip addr
```


> from https://blog.csdn.net/qq_41437512/article/details/125033563

> from https://multipass.run/docs
