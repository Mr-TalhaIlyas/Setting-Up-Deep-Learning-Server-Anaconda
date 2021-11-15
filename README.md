# Setting-Up-Deep-Learning-Server-Anaconda

## 1. Install GPU drivers

1st type the following command to get the list of recommend drivers for your PC.

```
ubuntu-drivers devices
```

s1.png)

Now install GPU drivers. I will install 470v drivers as shown in above images, so lets proceed.

```
// Update repository.  
$ sudo add-apt-repository ppa:graphics-drivers/ppa  
$ sudo apt update  

// Check recommeded driver can be used.  
$ apt-cache search nvidia | grep nvidia-driver-470 
```

s2.png)

Now lets install drivers using APT

```
// Install driver by apt.  
$ sudo apt-get install nvidia-driver-470  

// Reboot.  
$ sudo reboot  
```

※ During NVIDIA installation process if an error occurs or you can't proceed or you can't get your desired vesion to be displayes or run you have to uninstall it completelyl by

```
$ sudo apt --purge autoremove nvidia*
```
after installation verify it by

```
nvidia-smi
```
s3.png)

## 2. Install NVIDIA CUDA Toolkit

For using the Tensorflow or Pythorch we need to install the CUDA toolkit. Important thing here is that specific versions of both liberaries require a certain versions of CUDA nd cnDNN toolkit to be installed to be compatible.
* [Tensorflow Compatibality](https://www.tensorflow.org/install/gpu)
* [PyTorch Compatibality]()

As CUDA 11.3 is compatible with both so we will install that one.
Follow this [link](https://developer.nvidia.com/cuda-toolkit-archive) to get your desired version, and select appropriate options

s4.png)
The type command on the page or below.
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.3.0/local_installers/cuda-repo-ubuntu1804-11-3-local_11.3.0-465.19.01-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-11-3-local_11.3.0-465.19.01-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu1804-11-3-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda
```
The third command will download the files so it might take a while depending on the internet speed.
Next you need to set the Enviornment variables, in `~/.bashrc` so type

```
sudo gedit ~/.bashrc
```
you can use `vm` or other editors to but I am more comfertable with this one. Then add following lines at the end of opened window.

```

```

If everything went well you can check your CUDA version by typing.

```
nvcc --version
//or
nvcc -V
```
and you will see

s6.png)

If not you'll have to uninstall everything related to NVIDIA using command mentioned above and debug.

