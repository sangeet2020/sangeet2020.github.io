---
layout: post
title: Install K2-FSA/IceFall/Lhotse
date:  2023-08-11 16:40:16
description: Learn how to install K2-FSA, IceFall, and Lhotse on Linux/Ubuntu.
# tags: formatting links
categories: tutorial
---


## K2-FSA Installation

It is recommended to install K2-FSA in separate virtual environments. We use the recommended pre-compiled wheels.

- K2-CPU Installation (Torch version 2.0.1)
    ```bash
    pip install torch==2.0.1+cpu -f https://download.pytorch.org/whl/torch_stable.html
    pip install k2==1.24.3.dev20230718+cpu.torch2.0.1 -f https://k2-fsa.github.io/k2/cpu.html
    ```

- K2-GPU Installation (CUDA version 11.7)
    ```bash
    pip install torch==2.0.1+cu117 -f https://download.pytorch.org/whl/torch_stable.html
    pip install k2==1.24.3.dev20230718+cuda11.7.torch2.0.1 -f https://k2-fsa.github.io/k2/cuda.html
    ```

## IceFall Installation
Installation source: [IceFall Documentation](https://icefall.readthedocs.io/en/latest/installation/index.html#install-cuda-toolkit-and-cudnn)
```bash
git clone https://github.com/k2-fsa/icefall
cd icefall
pip install -r ./requirements.txt
export PYTHONPATH=$PWD:$PYTHONPATH
```

## Lhotse Installation
Installation source: [Lhotse Documentation](https://lhotse.readthedocs.io/en/latest/getting-started.html#installation)

```bash
git clone https://github.com/lhotse-speech/lhotse.git
pip install -e '.[dev]'
pre-commit install
pip install lhotse[kaldi]
```

## Installing CUDA (11.7) and cuDNN (8.9)
Installation source: [Icefall Documentation](https://k2-fsa.github.io/k2/installation/cuda-cudnn.html#cuda-11-7)

- Installing CUDA
    ```bash
    ## Choose installation dir
    INSTALL_PATH="/home/sagar/bin/cuda/11.7.1/"
    mkdir -p ${INSTALL_PATH}

    ## Download cuda
    wget https://developer.download.nvidia.com/compute/cuda/11.7.1/local_installers/cuda_11.7.1_515.65.01_linux.run

    chmod +x cuda_11.7.1_515.65.01_linux.run

    ./cuda_11.7.1_515.65.01_linux.run \
    --silent \
    --toolkit \
    --installpath=${INSTALL_PATH} \
    --no-opengl-libs \
    --no-drm \
    --no-man-page
    ```
- Installing cuDNN
    
    Check other available versions here: [cuDNN Archive by Nvidia](https://developer.nvidia.com/rdp/cudnn-archive)
    ```bash
    wget https://huggingface.co/csukuangfj/cudnn/resolve/main/cudnn-linux-x86_64-8.9.1.23_cuda11-archive.tar.xz

    tar xvf cudnn-linux-x86_64-8.9.1.23_cuda11-archive.tar.xz --strip-components=1 -C ${INSTALL_PATH}
```
- Set environment variables
    ```bash
    export CUDA_HOME=${INSTALL_PATH}
    export PATH=$CUDA_HOME/bin:$PATH
    export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=$CUDA_HOME/lib:$LD_LIBRARY_PATH

    export CUDA_TOOLKIT_ROOT_DIR=$CUDA_HOME
    export CUDA_TOOLKIT_ROOT=$CUDA_HOME
    export CUDA_BIN_PATH=$CUDA_HOME
    export CUDA_PATH=$CUDA_HOME
    export CUDA_INC_PATH=$CUDA_HOME/targets/x86_64-linux
    export CFLAGS=-I$CUDA_HOME/targets/x86_64-linux/include:$CFLAGS
    ``` 
- Verify installation
    ```bash
    which nvcc

    nvcc --version
    ```