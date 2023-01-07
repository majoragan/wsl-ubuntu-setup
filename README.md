# My Ubuntu setup in WSL
## Install Windows Subsystem for Linux

For Windows Subsystem for Linux (WSL2) installation follow the [official Microsoft instructions](https://learn.microsoft.com/en-us/windows/wsl/install). \
Also check the [Basic commands for WSL](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

<br/>

## Install Ubuntu 20.04 using WSL cli:

First, check all available Linux distributions online using PoweShell:
```POWERSHELL
wsl --list --online
```

then install your preferred distribution from the list (in this case `Ubuntu-20.04`):
```POWERSHELL
wsl --install -d <distribution>
```

and create new user & password once Ubuntu installation successfully finished. \ 
You're good to go.

## Ubuntu setup

User should be added into `sudo` group automatically, just enable execution of all `sudo` commands without password (optional, not recommended):
```BASH
sudo visudo
```
and just add `<user> ALL=(ALL) NOPASSWD=ALL` line to the file:
```BASH
#includedir /etc/sudoers.d
<user> ALL=(ALL) NOPASSWD=ALL
```

Update and upgrade installed repositories:
```BASH
sudo apt update && sudo apt upgrade -y
```

Install `gedit` to conveniently edit files using GUI editor (optional):
```BASH
sudo apt install gedit
```

##  systemd
To nable `systemd` edit `/etc/wsl.conf` file:
```BASH
sudo gedit /etc/wsl.conf
```
and add the following lines to the file:
```
[boot]
systemd=true
```

## PyEnv

## Docker

