[![OS: Linux](https://img.shields.io/badge/OS-Linux-2BBC8A?style=flat-square&logo=linux)](https://www.kernel.org/)
[![Shell: Bash](https://img.shields.io/badge/Shell-Bash-4EAA25?style=flat-square&logo=gnu-bash)](https://www.gnu.org/software/bash/)
[![Anaconda](https://img.shields.io/badge/Anaconda-2023.11-44A833?style=flat-square&logo=anaconda)](https://www.anaconda.com/)
[![License](https://img.shields.io/github/license/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda?style=flat-square)](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/LICENSE)
[![Last Commit](https://img.shields.io/github/last-commit/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda?style=flat-square)](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/commits/main)
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda&count_bg=%2379C83D&title_bg=%23555555&edge_flat=false)](https://hits.seeyoufarm.com)

# Setting-Up-Deep-Learning-Server-Anaconda
for setting deep learning server via Docker go [here](https://github.com/Mr-TalhaIlyas/Deep-Learning-Sever-via-Docker)
Assuming that you already have a machine runnning Linux/Ubuntu.

## 1. Install GPU drivers

1st type the following command to get the list of recommend drivers for your PC.

```
ubuntu-drivers devices
```

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s1.png)

Now install GPU drivers. I will install 470v drivers as shown in above images, so lets proceed.

```
// Update repository.  
$ sudo add-apt-repository ppa:graphics-drivers/ppa  
$ sudo apt update  

// Check recommeded driver can be used.  
$ apt-cache search nvidia | grep nvidia-driver-470 
```

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s2.png)

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
![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s3.png)

## 2. Install NVIDIA CUDA Toolkit

For using the Tensorflow or Pythorch we need to install the CUDA toolkit. Important thing here is that specific versions of both liberaries require a certain versions of CUDA nd cnDNN toolkit to be installed to be compatible.
* [Tensorflow Compatibality](https://www.tensorflow.org/install/gpu)
* [PyTorch Compatibality]()

As CUDA 11.3 is compatible with both so we will install that one.
Follow this [link](https://developer.nvidia.com/cuda-toolkit-archive) to get your desired version, and select appropriate options

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s4.png)
The type command on the page or below.
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.4.0/local_installers/cuda-repo-ubuntu1804-11-4-local_11.4.0-470.42.01-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804-11-4-local_11.4.0-470.42.01-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu1804-11-4-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda
```
The third command will download the files so it might take a while depending on the internet speed.

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s5.png)

Next you need to set the Enviornment variables, in `~/.bashrc` so type

```
sudo gedit ~/.bashrc
```
you can use `vm` or other editors to but I am more comfertable with this one. Then add following lines at the end of opened window.

```
#cuDNN path setup
export CUDA_HOME=/usr/local/cuda-11.4
export PATH=/usr/local/cuda-11.4/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.4/lib64:${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
# end
```
change `11.4` to your installed version.then 
```
sudo reboot
```

If everything went well you can check your CUDA version by typing.

```
nvcc --version
//or
nvcc -V
```
and you will see

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s6.png)

If not you'll have to uninstall everything related to NVIDIA using command mentioned above and debug. Also delete the files in `/usr/local` if any remaining.
```
sudo apt-get autoremove --purge cuda
```
## 3. Install cuDNN

You need to make an account on nvidia before downloading it. Each CUDA toolkit has its compatible cuDNN version so keep that in mind.
After logging in follow this [link](https://developer.nvidia.com/rdp/cudnn-archive) to download the cuDNN. I will download the cuDNN 8.2.4v as it is compatable with 11.4.

There are many ways to install cuDNN, I will show you one method which I think is easy.
Download the `cuDNN Runtime Library of Linux [x85_64]. Then `cd` to the download dir and type follwoing commands

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s7.png)

```
// This will extract all the files in the same dir
tar -xzvf <full ame of the file>.tgz
```
Then copy soem files to where the CUDA is installed by typing following
*in the newer versions the `lib64` might be replaced by just `lib` then just update the command by removing `64`and copy the files.*
```
sudo cp cuda/include/cudnn*.h /usr/local/cuda/include 
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64 
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```
Then check the cuDNN installation by typing.

```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```
The `cat` command might not output anything. But as long as it don't give error just proceed.

## 4. Install Anaconda

Download the file from [link](https://www.anaconda.com/products/distribution), and as conda installer is a bash script. To run the installation script, use the command after navigating (`cd`) download dir.
```
bash Anaconda3-2020.02-Linux-x86_64.sh
```
check the name of the downloaded file.

During Anaconda installation you might have to press `Enter` multiple times and it'll ask for multiple permision jsut go with the flow and allow default installation to proceed.
Then restart your terminal and you will see (base) at start of your username.

Then Verify that your shell's configuration file (e.g., .bashrc, .bash_profile, or .zshrc) contains the necessary lines to initialize Conda. Open the configuration file with a text editor and check for lines like:
```bash
# Anaconda3
export PATH="/home/your_username/anaconda3/bin:$PATH"
```
The reopen terminal and write `conda init`.

### 4.5. Create Conda `env`
 
 We will create two enviornments with conda one for tensorflow and one for pytorch.
 
For creating env type.
```
conda create -n <env_name> python=x.x
// activate by
conda activate  <env_name>
```
#### Tensorflow Installation

Then install tensorflow via `pip`

```
// first install pip via
sudo apt install python3-pip
// install tensorflow
pip install tensorflow-gpu==2.x.x
```
Test your installation by
```
python -c"import tensorflow as tf;print(tf.test_is_gpu_available())"
```
If it prints True and you can see the names of your GPUs and the memory in ouptput then you installation is successful.

#### PyTorch Installation

Install pytorch as 

```
conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
```
Then test the installation via

```
python -c"import torch;print(torch.cuda.is_available());print(torch.cuda.get_device_name())"
```
and it'll print out the name of one of the gpus in our machine

## 5. Starting the SSH service for Remote Connections
For starting the SSH server follow the steps below.

```
sudo apt update
sudo apt install openssh-server
```

When you install SSH, it runs automatically. You can check if SSH is running with the following command: 
```
sudo systemctl status ssh
```
if it shows active (running) then its running.

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s8.png)

If it is not running, run it with the following command.
```
sudo systemctl enable ssh
sudo systemctl start ssh
```
If you are using a firewall, make sure to allow ssh. If your firewall is disabled, you can ignore it.

```
sudo ufw allow ssh
```
Firewall is disabled by default, and you can check the status with the following command.
```
sudo ufw status
```
## Optional Port Change

You can also change to port of your SSH server if you want to for the type the following command

```
sudo gedit /etc/ssh/sshd_congfig
```
and locate line
```
# Port 22
```
Uncomment it and change the port number 
```
Port <new port>
// reboot system to take effect
sudo reboot
```
Restarting the SSH sever on Linux

```
sudo /etc/init.d/ssh restart
```
## Edit MOTD of SSH start screen


You need to edit two files:

1.   `/etc/motd` (Message of the Day)
2.   `/etc/ssh/sshd_config` here uncomment and change the setting `PrintLastLog` to `no`, this will disable the "Last login" message.

And then restart your sshd.

### Connect via MobaXterm

Welcome screen

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/s9.png)

## IDE

### Spyder
[Spyder](https://www.spyder-ide.org/) is the simplest, easiest IDE available for data science projects. If you are new to ML/DL then this is best IDE for you. For installation, after creating your `env` you can install Spyder via following command

```
conda instll spyder
```
and to use it just activate your env and type `spyder` in terminal.

### VS Code on server
For installing [VS Code](https://code.visualstudio.com/) on linux server via `snap` type

```
sudo snap install code --classic
```
and then from terminal run `code` and you are all set.

### Connect via VS Code from remote PC 

Follow instructions in this [BlogPost](https://jstar0525.tistory.com/14) to connect to your `ssh` server via VS code. It in korean so turn on google translator.

Just type `ssh username@your_ip_address` and press `enter`.

Then vs code will ask you for passworkd. If it gives error then it means your srever port is other than default port `22` then you have to specify the custom port by editing the `config` file of  vs code as below. Yor can edit that by click the dialogue box which opens when you enter your user name and ip.

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/config.png)
### Connect to VS code without Password
Follow steps here:
[Link](https://adamtheautomator.com/add-ssh-key-to-vs-code/)

## Display GPU stats in Compact view for mini-windows
Install `gpustat` via

```
pip install gpustat
// then to see type
gpustat -cp
```
you'll see follwoin compact view

![alt text](https://github.com/Mr-TalhaIlyas/Setting-Up-Deep-Learning-Server-Anaconda/blob/main/Pictures/stat.png)

or to continuously watch type

```
watch -c gpustat -cp --color
```


Reference:

https://gist.github.com/denguir/b21aa66ae7fb1089655dd9de8351a202
https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#network-repo-installation-for-ubuntu
