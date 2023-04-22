---
layout: post
title:  Installation without sudo
date:   2022-11-12 16:40:16
description: Install linux/ubuntu packages without admin rights
tags: formatting links
categories: tutorial
---
## Installation of Python without sudo
1. Download python
```
wget https://www.python.org/ftp/python/3.5.6/Python-3.5.6.tgz
```
2. Decompress
```
tar -xzvf Python-3.5.6.tgz
```
3. Let us suppose your destination directory for python installation is `/netscratch/sagar/usr/share/python3.5.6/`, so you would need to enter

    ```
    ./configure --prefix=/netscratch/sagar/usr/share/python3.5.6/
    ```
4. Compile, build and install
```
make -j 8
make -j 8 install
```

## Create the Virtual Environment
- 
```bash
python3.5 -m venv sb_env
source /netscratch/sagar/thesis/sb_env/bin/activate
pip3 install torch==1.11.0 torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu113
```

## Installation of other packages without sudo
1. Let's suppose you want to install `tree` package without sudo.

    ```bash
    wget http://mama.indstate.edu/users/ice/tree/src/tree-2.0.2.tgz
    tar -xzvf tree-2.0.2.tgz
    ./configure --prefix=/home/sagar/.local/
    make PREFIX=/home/sagar/.local/ MANDIR=/home/sagar/.local/share/ install && chmod -v 644 /home/sagar/.local/share/
    ```

2. Let's suppose you want to install `sox` package without sudo.
Here `/netscratch/sagar/usr/share/sox` is the destination dir.

    ```bash
    wget https://nav.dl.sourceforge.net/project/sox/sox/14.4.2/sox-14.4.2.tar.bz2
    tar -xf sox-14.4.2.tar.bz2
    ./configure --prefix=/netscratch/sagar/usr/share/sox
    make -s && make install
    ```