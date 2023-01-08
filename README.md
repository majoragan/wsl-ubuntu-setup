# My Ubuntu setup in WSL
## Install Windows Subsystem for Linux

For Windows Subsystem for Linux (WSL2) installation follow the [official Microsoft instructions](https://learn.microsoft.com/en-us/windows/wsl/install). \
Also check the [Basic commands for WSL](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)

<br>

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

<br>

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

<br>

##  systemd

To enable `systemd` edit `/etc/wsl.conf` file:

```BASH
sudo gedit /etc/wsl.conf
```

and add the following lines to the file:

```BASH
[boot]
systemd=true
```

<br>

## pyenv

For managing multiple Python versions for different projects without need of `Docker` I use `pyenv` package. \
First, install `pyenv` dependencies:

```BASH
sudo apt install build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

you can check the official [pyenv wiki page](https://github.com/pyenv/pyenv/wiki) for list of dependencies for your preffered distribution.

<br/>

then, use [pyenv automatic installer](https://github.com/pyenv/pyenv/blob/master/README.md#automatic-installer) to install `pyenv`:

```BASH
curl https://pyenv.run | bash
``` 

and then set up your `bash` shell environment for `pyenv` appending the following lines to the end of `~/.bashrc` file running this in terminal:

```BASH
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

<br/>

## Docker

Clear out any residual `Docker` installs from the system installed before:

```BASH
sudo apt remove docker docker-engine docker.io containerd runc
```

then install dependencies:

```BASH
sudo apt install --no-install-recommends apt-transport-https ca-certificates curl gnupg2
```

source your OS-specific variables:

```BASH
. /etc/os-release
```

add `docker` repoository to apt list of trusted packages:

```BASH
curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo apt-key add -
```

and update the `docker` repository information so `apt` can use it in future:

```BASH
 echo "deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/docker.list
 ```

 Now we can update the repositories:

 ```BASH
 sudo apt update
 ```

and then install `Docker`:

```BASH
sudo apt install docker-ce docker-ce-cli containerd.io
```

Next step is to add `<user>` to the `docker` group:

```BASH
sudo usermod -aG docker $USER
```
Add this to `visudo` file for Docker passwordless launch:

```BASH
sudo visudo
```

```BASH
#Docker
%docker ALL=(ALL)  NOPASSWD: /usr/bin/dockerd
```

Prepare shared directory:

```BASH
DOCKER_DIR=/mnt/wsl/shared-docker
mkdir -pm o=,ug=rwx "$DOCKER_DIR"
chgrp docker "$DOCKER_DIR"
```

[SOURCE](https://dev.to/bowmanjd/install-docker-on-windows-wsl-without-docker-desktop-34m9)

<br/>

## Starship prompt