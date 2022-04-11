---
title: "Installation Guide for TP-link AC1300 Adapter - Archer T3U in Ubuntu 20"
date: 2022-1-11T23:07:37+08:00
draft: false
categories: IT
---

# Devices

- TP-link AC1300 Mini Wireless MU-MIMO USB Adapter - Archer T3U
- One offline PC: Ubuntu 20 - 5.11.0 - 27 - generic, new installed
- One online PC: Ubuntu 20 - 5.11.0 - 38 - generic

# Solution

following instruction from [tp-link community](https://community.tp-link.com/en/home/forum/topic/208022)

```bash
git clone https://github.com/cilynx/rtl88x2bu.git
cd rtl88x2bu
VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)
sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}
sudo dkms add -m rtl88x2bu -v ${VER}
sudo dkms build -m rtl88x2bu -v ${VER}
sudo dkms install -m rtl88x2bu -v ${VER}
sudo modprobe 88x2bu
```

from this, we know we need to prepare git repository (download from online pc), dkms package (apt-offline)

# Steps

## 1. install `apt-offline`

### installl in online PC

run `sudo apt install apt-offline`

### install in offline PC

1. download `.deb` file with right ubuntu version from https://pkgs.org/download/apt-offline
   - click into the link, find the link from `Download` (e.g. http://archive.ubuntu.com/ubuntu/pool/universe/a/apt-offline/apt-offline_1.8.2-1_all.deb)
   - download this file use `wget`
2. check its `requires` at the same page
   - check if the package in `requires` exists in the offline PC
   - if not, download the `.deb` like the `apt-offline`
3. use a thumb driver to copy them and mount it to offline PC
4. run `sudo dpkg -i *.deb` in the folder with the `.deb` you just downloaded
5. if error occurs in installation, may need to restart this step

## 2. Update, Upgrade, Install `dkms`

### Update

1. after installing `apt-offline`, run `sudo apt-offline set update.sig --update` to get the `.sig` file.
2. move the `.sig` file from offline PC to online PC using thumb drive
3. run `sudo apt-offline get update.sig --threads 2 --bundle update.zip` with 2 threading
4. move the `update.zip` to offline PC
5. run `sudo apt-offline install update.zip` and `sudo apt update`

### Upgrade

1. after update done,  run `sudo apt-offline set upgrade.sig --upgrade` to get the `.sig` file.
2. move the `.sig` file from offline PC to online PC using thumb drive
3. run `sudo apt-offline get upgrade.sig --threads 2 --bundle upgrade.zip` with 2 threading
4. move the `upgrade.zip` to offline PC
5. run `sudo apt-offline install upgrade.zip` and `sudo apt upgrade`
6. a error may occur waring about `--fix-missing`
7. run `sudo apt upgrade --fix-missing`

### 3. Install dkms

1. in offline PC, run `sudo apt-offline set dkms.sig --install-package dkms`
2. move the `.sig` file from offline PC to online PC using thumb drive
3. run `sudo apt-offline get dkms.sig --threads 2 --bundle dkms.zip` with 2 threading
4. move the `dkms.zip` to offline PC
5. run `sudo apt-offline install dkms.zip` and `sudo apt install dkms`

### 4. Git pull

1. in online PC, run `git clone https://github.com/cilynx/rtl88x2bu.git`
2. mv the download folder to offlin PC

### 5. Follow the instruction

```
1. cd rtl88x2bu
2. VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)
3. sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}
4. sudo dkms add -m rtl88x2bu -v ${VER}
5. sudo dkms build -m rtl88x2bu -v ${VER}
6. sudo dkms install -m rtl88x2bu -v ${VER}
7. sudo modprobe 88x2bu
```
## 3. Plugin Adapter

after finish 1 and 2, just plug in the adapter, and it can work now
