# UBUNTU <!-- omit in toc -->

- [ACTIONS](#actions)
  - [check system log files](#check-system-log-files)
  - [mount smb share (ubuntu 18.04)](#mount-smb-share-ubuntu-1804)
  - [Create desktop shortcut](#create-desktop-shortcut)
    - [with gui](#with-gui)
    - [with terminal commands](#with-terminal-commands)
  - [Display shortcuts](#display-shortcuts)
  - [Install drivers for Huion pen tablet H950P](#install-drivers-for-huion-pen-tablet-h950p)
  - [Setup Samsung printer](#setup-samsung-printer)
  - [show git branch in terminal](#show-git-branch-in-terminal)
  - [repair file system](#repair-file-system)
  - [format USB Drives](#format-usb-drives)
    - [Install utility parted](#install-utility-parted)
    - [Find the device name](#find-the-device-name)
    - [Format with EXT4](#format-with-ext4)
    - [Format with FAT32](#format-with-fat32)
  - [update label on FAT partition (USB drive) with fatlabel](#update-label-on-fat-partition-usb-drive-with-fatlabel)
  - [install VPN certificate](#install-vpn-certificate)
  - [set proxy](#set-proxy)
  - [add command alias](#add-command-alias)
  - [update PATH environment variable](#update-path-environment-variable)
  - [configure grub2 boot lader](#configure-grub2-boot-lader)
- [APPs](#apps)
  - [Network utils](#network-utils)
  - [ranger, console file manager](#ranger-console-file-manager)
  - [tmux, a terminal multiplexer](#tmux-a-terminal-multiplexer)
  - [peek, animated gif screen recorder](#peek-animated-gif-screen-recorder)
  - [freefilesync, compare & sync files](#freefilesync-compare--sync-files)
  - [Configure Grub2](#configure-grub2)
  - [OpenSSL](#openssl)
    - [create csr](#create-csr)
    - [check csr](#check-csr)
    - [convert certificate (mycertificate.cer) to pem file that will be used to openconnect](#convert-certificate-mycertificatecer-to-pem-file-that-will-be-used-to-openconnect)
  - [OpenConnect](#openconnect)
    - [example](#example)
  - [jq](#jq)
    - [installation](#installation)
    - [usage](#usage)
  - [Cairo Dock](#cairo-dock)
    - [installation](#installation-1)
  - [CISCO AnyConnect](#cisco-anyconnect)
  - [Firefox Developer Edition](#firefox-developer-edition)
  - [Node.js](#nodejs)
- [WSL](#wsl)

## ACTIONS

### check system log files

- /var/log/syslog
- /var/log/auth.log

### mount smb share (ubuntu 18.04)

[Reference](https://wiki.ubuntu.com/MountWindowsSharesPermanently)

- Install package cifs `sudo apt-get install cifs-utils`
- Create mount folder `sudo mkdir /mnt/PI`
- Mount the share `sudo mount -t cifs //192.168.1.5/PI /mnt/PI -o user=pi`
- Create file ~/.smbcredentials and add following lines
  
  ```bash
  username=pi
  password=XXXXX
  ```
- Edit file /etc/fstab and add following line at the end

  ```bash
  //192.168.1.5/PI /mnt/PI cifs uid=teo,gid=teo,credentials=/home/teo/.smbcredentials,defaults 0 0
  ```
- Run command `sudo mount -a` to mount everything in fstab

To mount to a windows share with domain credentials e.g. ProdsAppsSD storage of companyname then

- create mount folder `sudo mkdir /mnt/ProdsAppsSD`
- add in file /etc/fstab line `//develfs/devel/ProdsAppsSD /mnt/ProdsAppsSD cifs uid=teo,gid=teo,username=t.kosmopoulos,password=******,domain=HLX,vers=2.0,defaults 0 0`

a very important detail here is parameter `vers`, check [here](hhttps://superuser.com/a/1157945) for more information

### Create desktop shortcut

#### with gui

[Reference](https://askubuntu.com/questions/1092879/how-to-create-a-link-to-a-folder-in-ubuntu-18-04-using-gui)

- Drag the folder to desktop while holding the Ctrl+Shift button

#### with terminal commands

### Display shortcuts

- Settings > Devices > Keyboard

### Install drivers for Huion pen tablet H950P

[H950P](https://www.huion.com/pen_tablet/Inspiroy/H950P.html)

- Download drivers from [DIGImend](https://github.com/DIGImend/digimend-kernel-drivers)
  - Clone repo <https://github.com/DIGImend/digimend-kernel-drivers.git>

    ```bash
    mkdir DigimendKernelDrivers
    cd DigimendKernelDrivers
    git clone https://github.com/DIGImend/digimend-kernel-drivers.git .
    ```

- Build & install drivers
  - Build drivers, run command `make`
  - Install drivers, run command `sudo make install`
  
    After each kernel upgrade run command `make clean` and then the above commands

### Setup Samsung printer

[Reference](https://askubuntu.com/a/977851)

### show git branch in terminal

[Reference](https://www.shellhacks.com/show-git-branch-terminal-command-prompt/)

- run `echo $PS1` and use the output in the lines added in .bashrc (see below) i.e. `${debian_chroot:+($debian_chroot)}\u@\h:\w\$`
- add lines in file `~/.bashrc`
  
  ```bash
  git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
  }

  export PS1="${debian_chroot:+($debian_chroot)}\u@\h:\w\$(git_branch)\$ "
  ```

- load .bashrc by running `source ~/.bashrc` to apply changes for the current session
- for colorizing git branch `export PS1="${debian_chroot:+($debian_chroot)}\u@\h:\w\[\033[00;32m\]\$(git_branch)\[\033[00m\]\$ "`
  
  <https://www.shellhacks.com/bash-colors/>

### repair file system

[Reference](https://help.ubuntu.com/community/FilesystemTroubleshooting)

-- `fsck.vfat -n /dev/sda1`, check FAT32 device /dev/sda1

### format USB Drives

[Reference](https://linuxize.com/post/how-to-format-usb-sd-card-linux/)

#### Install utility parted

Check for utility **parted** by executing command `parted -v`

#### Find the device name

- To find the device name
  - insert USB drive and execute command `lsblk`
  - **OR** execute command `dmesg` and insert USB drive

Let's assume that our USB drive device name is /dev/sdb.

#### Format with EXT4

- Create a GPT partition table with command `sudo parted /dev/sdb --script -- mklabel gpt`
- Create a EXT4 partition that takes the whole space with command `sudo parted /dev/sdb --script -- mkpart primary ext4 0% 100%`
- Format the partition to ext4 with command `sudo mkfs.ext4 -F /dev/sdb1`
- Verify by printing partition table with command `sudo parted /dev/sdb --script print`

#### Format with FAT32

- create the partition table with command `sudo parted /dev/sdb --script -- mklabel msdos`
- create a Fat32 partition that takes the whole space `sudo parted /dev/sdb --script -- mkpart primary fat32 1MiB 100%`
- format the boot partition to FAT32 and set label 'INTENSO' with command `sudo mkfs.vfat -F32 -n INTENSO /dev/sdb1`
- verify by printing partition table with command `sudo parted /dev/sdb --script print`

### update label on FAT partition (USB drive) with fatlabel

- show label for device /dev/sda1 `sudo fatlabel /dev/sda1`
- set label for device /dev/sda1 `sudo fatlabel /dev/sda1 INTENSO`

### install VPN certificate

- create server private
- generate csr

### set proxy

To set proxy in users current session, execute commands:

- export http_proxy=http://proxy.companyname.gr:8080
- export https_proxy=http://proxy.companyname.gr:8080

To set proxy permanently for current user

- add the following lines at the end of file ~/.bashrc:

```bash
export http_proxy=http://proxy.companyname.gr:8080
export https_proxy=http://proxy.companyname.gr:8080
```

- reload file .bashrc by executing `source ~/.bashrc`

To set system proxy (for all users)

- create `.sh` file (e.g. athex_proxy.sh) in folder /etc/profile.d with contents

      export http_proxy='http://proxy.companyname.gr:8080'
      export https_proxy='http://proxy.companyname.gr:8080'
      export no_proxy='127.0.0.1,localhost,t-idm-1.companyname.gr'
      export HTTP_PROXY="http://proxy.companyname.gr:8080/"
      export HTTPS_PROXY="http://proxy.companyname.gr:8080/"
      export NO_PROXY='127.0.0.1,localhost,t-idm-1.companyname.gr'
- restart

To set proxy for apt install

[Reference](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-the-proxy-for-apt-for-ubuntu-18-04/)

- create file proxy.conf `sudo touch /etc/apt/apt.conf.d/proxy.conf`
- add following lines in file proxy.conf

        Acquire::http::Proxy "http://proxy.companyname.gr:8080/";
        Acquire::https::Proxy "http://proxy.companyname.gr:8080/";

### add command alias

- open file ~/.bashrc
- add line `alias command_name="command"` e.g. `alias projects="cd ~/Projects"`
- reload file .bashrc with command `source ~/.bashrc`
- execute command `command_name` e.g. `projects`

### update PATH environment variable

To update current user's PATH variable with line `/home/teo/Apps/mongosh/bin`

- add line `export PATH=/home/teo/Apps/mongosh/bin:$PATH` in file `~/.bashrc`
- reload bashrc by running command `source ~/.bashrc`

To update system's PATH variable with line `/home/teo/Apps/mongosh/bin`

- add line `:/home/teo/Apps/mongosh/bin` in file `/etc/environment`
- reload file `/etc/environment` by running command `source /etc/environment`

### configure grub2 boot lader

[Reference](https://www.howtogeek.com/196655/how-to-configure-the-grub2-boot-loaders-settings/)

## APPs

### Network utils

- install traceroute `sudo apt-get install traceroute`

### ranger, console file manager

[Refernce](https://github.com/ranger/ranger)

### tmux, a terminal multiplexer

[Reference](https://github.com/tmux/tmux)

- Commands
  - `Ctrl-B %` for vertical split
  - `Ctrl-B "` for horizontal split
  - `Ctrl-B O` to change active shell
  - `Ctrl-B ?` for help
  - `Ctrl-B d` detach from tmux terminal
  - `Ctrl-B [` to use normal navigation keys (e.g. upArrow, pgDown) and then `q` to quit this mode
  - `tmux attach` to reentet tmux terminal
- Comparison with other `split -terminal` apps
  - <https://opensource.com/article/20/5/split-terminal>
  - <https://linuxhint.com/tmux_vs_screen/>

### peek, animated gif screen recorder

[Reference](https://github.com/phw/peek)

Install by executing the following commands:

```bash
sudo add-apt-repository ppa:peek-developers/stable
sudo apt update
sudo apt install peek
```

### freefilesync, compare & sync files

[Reference](https://freefilesync.org/)

Download package file from <https://freefilesync.org/download.php>, then execute the following commands:

```bash
cd Downloads/
sudo tar xvf FreeFileSync_*.tar.gz -C /opt/
cd /opt/FreeFileSync
```

Create desktop launcher by doing the following

- cp /opt/FreeFileSync/FreeFileSync.desktop ~/Desktop/
- edit FreeFileSync.desktop
  - update path in params `Exec` & `Icon`
  - add at the beginning of file, line `#!/usr/bin/env xdg-open`
- run ~/Desktop/FreeFileSync.desktop and select `Trust & launch`

More on <https://www.tecmint.com/freefilesync-compare-synchronize-files-in-ubuntu/>

### Configure Grub2

[Reference](https://help.ubuntu.com/community/Grub2)

- modify file /etc/default/grub
- execute command `sudo update-grub`
- restart `shutdown -r now`

### OpenSSL

[Reference](https://help.ubuntu.com/community/OpenSSL)

#### create csr

```bash
openssl req -nodes -newkey rsa:2048 -keyout u.name.key \
        -out t.kosmopoulos.csr -subj "/CN=T\.Kosmopoulos/emailAddress=t\.kosmopoulos@companyname\.gr"
```

#### check csr

```bash
openssl req -text -in t.kosmopoulos.csr -noout -verify
```

#### convert certificate (mycertificate.cer) to pem file that will be used to openconnect

```bash
openssl x509 -inform der -in mycertificate.cer -outform PEM -out t.kosmopoulos.pem
```

### OpenConnect

[Reference](https://www.infradead.org/openconnect/manual.html)

#### example

```bash
openconnect --user t.kosmopoulos --usergroup ATHGroupRA --certificate /home/teo/Documents/CERTIFICATES/t.kosmopoulos.pem --sslkey /home/teo/Documents/CERTIFICATES/server.key --servercert sha256:*************************************************** --verbose --passwd-on-stdin vpn.server.com
```

### jq

lightweight and flexible command-line JSON processor

[Reference] <https://stedolan.github.io/jq/>

#### installation

```apt-get install jq```

#### usage

```cat ~/.config/google-chrome/Local\ State | jq .profile```

### Cairo Dock

A desktop interface that takes the shape of docks, desklets, panel, etc

[url] <https://glx-dock.org/>

#### installation

```bash
sudo add-apt-repository ppa:cairo-dock-team/ppa
sudo apt-get update
sudo apt-get install cairo-dock cairo-dock-plug-ins
```

### CISCO AnyConnect

[installation instructions]<https://www.cisco.com/c/en/us/support/docs/security/anyconnect-secure-mobility-client/214612-configure-anyconnect-secure-mobility-cli.html>

- client certificate (t.kosmopoulos.pem) location: /home/teo/.cisco/certificates/client/
- private certificate (t.kosmopoulos.key) location: /home/teo/.cisco/certificates/client/private/
- connection steps
  - run executable file: /opt/cisco/anyconnect/bin/vpn
  - insert command `connect irida.companyname.gr`
  - insert user credentials
- profiles location: `/opt/cisco/anyconnect/profile`

### Firefox Developer Edition

[installation](https://askubuntu.com/a/548005)

- download from [here](https://www.mozilla.org/en-US/firefox/developer/)
- extract files & copy to folder /opt/firefox_dev

### Node.js

[upgrade with Node's version manager n](https://phoenixnap.com/kb/update-node-js-version#htoc-option-2-update-nodejs-with-npm-node-package-manager)

Invesigate alternative node.js version manager [n-install](https://github.com/mklement0/n-install)

Linux system paths of `node`, `n`

- /home/teo/.npm-global/bin/n
- /usr/bin/node
- /usr/local/bin/npm

To upgrade using `n` execute command `n v12.20.1`

**Command Result**

```bash
installing : node-v12.20.1
       mkdir : /usr/local/n/versions/node/12.20.1
       fetch : https://nodejs.org/dist/v12.20.1/node-v12.20.1-linux-x64.tar.xz
   installed : v12.20.1 (with npm 6.14.10)

Note: the node command changed location and the old location may be remembered in your current shell.
         old : /usr/bin/node
         new : /usr/local/bin/node
To reset the command location hash either start a new shell, or execute PATH="$PATH"
```

## WSL

[Installation guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

Follow instructions [here](https://gist.github.com/noygal/6b7b1796a92d70e24e35f94b53722219) for wsl and [nvm](https://github.com/nvm-sh/nvm) installation.

### redirect node.js app traffic to fiddler proxy (running in windows)

1. Get WSL interface IP wsl's network adapter in Windows
2. Setup proxy in user's current session with commands:
```
export http_proxy=http://wsl_interface_ip.gr:8080
export https_proxy=http://wsl_interface_ip:8080
```
3. run node.js app with envrionment variable `env NODE_TLS_REJECT_UNAUTHORIZED=0` 


## GIT

### Setup hook prepare-commit-msg

Execute the following command from the root folder of your project, [reference](https://bitbucket.org/snippets/atlassian/qedp7d#comment-3870553)
```
curl https://gist.githubusercontent.com/jared-christensen/f9db573183ba76c12b6125eda6125ebc/raw/e02ab3b97ac930ab5c2ddc343fb54ebddd85ee96/prepare-commit-msg.sh > .git/hooks/prepare-commit-msg && chmod u+x .git/hooks/prepare-commit-msg
```