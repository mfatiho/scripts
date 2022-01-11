# Requirements

Before starting the installation, make sure you have installed [nvidia-driver](https://www.nvidia.com/en-us/drivers/unix), [cuda toolkit and cuDNN](https://github.com/mfatiho/scripts/tree/main/cudatoolkit-cudnn/ubuntu20.04).

# Installation steps

1. Install following packages

        sudo apt install build-essential cmake pkg-config unzip yasm git checkinstall
        sudo apt install libjpeg-dev libpng-dev libtiff-dev
        sudo apt install libavcodec-dev libavformat-dev libswscale-dev libavresample-dev
        sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
        sudo apt install libxvidcore-dev x264 libx264-dev libfaac-dev libmp3lame-dev libtheora-dev
        sudo apt install libfaac-dev libmp3lame-dev libvorbis-dev
        sudo apt install libopencore-amrnb-dev libopencore-amrwb-dev
        sudo apt install libgtk-3-dev
        sudo apt install libtbb-dev
        sudo apt install libatlas-base-dev gfortran
        sudo apt install libprotobuf-dev protobuf-compiler
        sudo apt install libgoogle-glog-dev libgflags-dev
        sudo apt install libgphoto2-dev libeigen3-dev libhdf5-dev doxygen

2. Install opencv 4.5.5 on GPU (RTX 3060)

        mkdir opencvbuild && cd opencvbuild
        wget -O opencv.zip https://codeload.github.com/opencv/opencv/zip/refs/tags/4.5.5
        wget -O opencv_contrib.zip https://codeload.github.com/opencv/opencv_contrib/zip/refs/tags/4.5.5
        unzip opencv.zip
        unzip opencv_contrib.zip
        mv opencv-4.5.5 opencv
        mv opencv_contrib-4.5.5 opencv_contrib

        mkdir build && cd build
        
        export CONDA_ENV=~/miniconda3/envs/[CONDA ENVIROMENT]
        
        cmake -D CMAKE_BUILD_TYPE=RELEASE \
            -D CMAKE_INSTALL_PREFIX=/usr/local \
            -D WITH_TBB=ON \
            -D ENABLE_FAST_MATH=1 \
            -D CUDA_FAST_MATH=1 \
            -D WITH_CUBLAS=1 \
            -D WITH_CUDA=ON \
            -D BUILD_opencv_cudacodec=OFF \
            -D WITH_CUDNN=ON \
            -D OPENCV_DNN_CUDA=ON \
            -D CUDA_ARCH_BIN=8.6 \
            -D WITH_V4L=ON \
            -D WITH_QT=OFF \
            -D WITH_OPENGL=ON \
            -D WITH_GSTREAMER=ON \
            -D OPENCV_GENERATE_PKGCONFIG=ON \
            -D OPENCV_PC_FILE_NAME=opencv.pc \
            -D OPENCV_ENABLE_NONFREE=ON \
            -D OPENCV_PYTHON3_INSTALL_PATH=$CONDA_ENV/lib/python3.9/site-packages \
            -D PYTHON_EXECUTABLE=$CONDA_ENV/bin/python \
            -D OPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules \
            -D INSTALL_PYTHON_EXAMPLES=OFF \
            -D INSTALL_C_EXAMPLES=OFF \
            -D BUILD_EXAMPLES=OFF \
            ../opencv

        make -j$(nproc)
        sudo make install
        sudo /bin/bash -c 'echo "/usr/local/lib" >> /etc/ld.so.conf.d/opencv.conf'
        sudo ldconfig

        # Conda set library
        ln -s /usr/local/lib/python3.9/site-packages/cv2 $CONDA_ENV/lib/python3.9/site-packages/cv2
