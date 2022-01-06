# Requirements

Before starting the installation, make sure you have installed [nvidia-driver](https://www.nvidia.com/en-us/drivers/unix)

# Installation steps

1. Install **cuda toolkit**. For the 11.4 cuda toolkit version, you can run the following codes in order. If you are going to choose another version, you can get help from the [cuda toolkit download page](https://developer.nvidia.com/cuda-downloads).

        wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
        sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
        wget https://developer.download.nvidia.com/compute/cuda/11.4.3/local_installers/cuda-repo-ubuntu2004-11-4-local_11.4.3-470.82.01-1_amd64.deb
        sudo dpkg -i cuda-repo-ubuntu2004-11-4-local_11.4.3-470.82.01-1_amd64.deb
        sudo apt-key add /var/cuda-repo-ubuntu2004-11-4-local/7fa2af80.pub
        sudo apt-get update
        sudo apt-get -y install cuda

2. Install **cuDNN**. For the 11.4 cuda version, you can run the following codes in order.
    1. [Download packages](https://developer.nvidia.com/rdp/cudnn-download)
        1. libcudnn8_8.2.4.15-1+cuda11.4_amd64.deb
        1. libcudnn8-dev_8.2.4.15-1+cuda11.4_amd64.deb
        1. libcudnn8-samples_8.2.4.15-1+cuda11.4_amd64.deb

    2. Installation deb packages
            
            sudo dpkg -i libcudnn8_8.2.4.15-1+cuda11.4_amd64.deb
            sudo dpkg -i libcudnn8-dev_8.2.4.15-1+cuda11.4_amd64.deb
            sudo dpkg -i libcudnn8-samples_8.2.4.15-1+cuda11.4_amd64.deb

3. Add following lines to .bashrc files

        export CUDA=11.4
        export PATH=/usr/local/cuda-$CUDA/bin${PATH:+:${PATH}}
        export CUDA_PATH=/usr/local/cuda-$CUDA
        export CUDA_HOME=/usr/local/cuda-$CUDA
        export LIBRARY_PATH=$CUDA_HOME/lib64:$LIBRARY_PATH
        export LD_LIBRARY_PATH=/usr/local/cuda-$CUDA/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
        export LD_LIBRARY_PATH=/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH
        export NVCC=/usr/local/cuda-$CUDA/bin/nvcc
        export CFLAGS="-I$CUDA_HOME/include $CFLAGS"
        
4. Test the installation of cuDNN

        cp -r /usr/src/cudnn_samples_v8/ $HOME

        cd $HOME/cudnn_samples_v8/mnistCUDNN/
        make
        ./mnistCUDNN