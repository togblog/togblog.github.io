---
published: true
---
Setting up bundled-up source code- installing its dependencies, building makefiles, and finally running tests to make sure it all works- can be a real pain. Recently I've gone through this ordeal again on my Optimus-enabled Linux partition, but could not find the original documentation I created five years ago. This post is mostly a reference for future me who probably wants to install CUDA-reliant applications on their other machines, but it’s also for those struggling in the same way.

**"Value compute_x is not defined for ‘gpu_architecture"**

In brevity it just means that for whatever NVIDIA graphics card (or CUDA version) you have, the current compute capability value isn’t supported. Which means you have to change it to a value that is supported by your hardware- usually this parameter is located in some sort of makefile in the vicinity of a “gpu_architecture” variable. Ensure you have the correct version of CUDA installed, and look at the list of supported GPUs, find the corresponding compute capability number for your specific graphics card, and replace it in the field.

**"Error: libcudart.so.x: cannot open shared file"**

The shared library cache couldn’t find the CUDA files, so you’ll need to point it to the right direction. Type in

```
sudo ldconfig /usr/local/cuda-x/lib64
```
or wherever your CUDA files are stored on your machine.

**"…code=38(cudaErrorNoDevice)”**

For whatever reason your NVIDIA GPU (with sufficient compute capability) isn’t being detected, which is due to a number of reasons. Entering

```
nvidia-smi
```

can usually give you more insight into the causes. If it provides you with less than an obvious answer (i.e. the correct graphics card you’re using), it’s worth investigating your drivers and/or CUDA installation.

**“CUDA driver version is insufficient for CUDA runtime”**

The error is pretty explanatory, but if you’re certain that you have the correct drivers installed properly (determine the appropriate driver version on the NVIDIA site) and up to date for your CUDA version, it’s likely an issue with improper symlinks. Go to where your drivers are installed (“lib” for 32-bit, “lib64” for 64-bit), and create a symlink:

```
sudo ln -s libcuda.so.1 libcuda.so
sudo ldconfig
```

This post will be updated if I run into any more errors that I probably won't remember in the future.
