---
layout: post
title:  Installation without sudo
date:   2022-11-12 16:40:16
description: Install linux/ubuntu packages without admin rights
# tags: formatting links
categories: tutorial
---
## Installing Python from Scratch Without sudo

In this tutorial, we'll guide you through the process of installing Python from scratch without using `sudo`. This approach allows you to have a self-contained Python installation in your user directory, giving you more control over your Python environment.

- Step 1: Download Python
    ```bash
    VERSION=3.10.12
    USERNAME=spongebob
    wget https://www.python.org/ftp/python/$VERSION/Python-$VERSION.tgz
    tar -xf Python-$VERSION.tgz
    ```

- Step 2: Configure and Install
    ```bash
    cd Python-${VERSION}
    mkdir /home/${USERNAME}/.localpython/${VERSION}/
    ./configure --prefix=/home/${USERNAME}/.localpython/${VERSION}/ --enable-optimizations
    make -j 12
    make -j 12 install
    ```

- Step 3: Create Symbolic Links
    ```bash
    ln -s /home/${USERNAME}/.localpython/3.10.12/bin/python3.10 /home/${USERNAME}/.localpython/3.10.12/bin/python
    ln -s /home/${USERNAME}/.localpython/3.10.12/bin/pip3 /home/${USERNAME}/.localpython/3.10.12/bin/pip
    ```

- Step 4: Update PATH in bashrc
    Add the following line to your `~/.bashrc` or `~/.bash_profile` file to ensure your new Python installation is in your PATH:
    ```bash
    export PATH="/home/${USERNAME}/.localpython/3.10.12/bin:$PATH"
    ```

## Installing Python using Miniconda
```bash
## this installs the version 3.11.4
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh -b -p miniconda

export PATH="/mnt/users/sagarst/bin/miniconda/bin:$PATH"
export LD_LIBRARY_PATH=/mnt/users/sagarst/bin/miniconda/lib:$LD_LIBRARY_PATH
```

## Create the Virtual Environment
```bash
python -m venv sb_env
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