# tutorial
ubuntu16.10+cuda8.0+cudnn8.0+opencv3.2+caffe+tensorflow


安装Nvidia显卡驱动：
1.在ubuntu下，安装的不一样，打开终端，输入sudo apt-get install　linux-headers-generic，安装好之后再安装驱动。
2.安装CUDA9.0

　　首先去官网下载CUDA 9.0，这里因为写该博客时NVIDIA网站进不去了，就截了一个图，下载的是.run文件

　　

　　下载完CUDA 9.0之后执行如下语句，运行.run文件

　　　　sudo sh cuda_9.0.176_384.98_linux.run

　　单击回车，一路往下运行，直到提示“是否为NVIDIA安装驱动nvidia-384？”，选择否，因为已经安装好驱动程序了，其他的全都是默认，不过要记住安装位置，默认是安装在/usr/local/cuda文件夹下。

　　配置环境变量，运行如下命令打开profile文件

　　　　sudo gedit  /etc/profile

　　打开文件后在文件末尾添加路径，也就是安装目录，命令如下：

　　　　export  PATH=/usr/local/cuda-9.0/bin:$PATH

　　　　export  LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64$LD_LIBRARY_PATH　　

 　　保存，然后重启电脑

　　　　sudo reboot

3.测试CUDA的Samples例子

　　cd  /usr/local/cuda-9.0/samples/1_Utilities/deviceQuery

　　　sudo make

　　　./deviceQuery

　　如果显示的是关于GPU的信息，则说明安装成功了。


如遇到登录界面无限循环，则表示显卡驱动未装好。则：
sudo apt-get remove --purge nvidia-*
sudo apt-get install ubuntu-desktop
sudo rm /etc/X11/xorg.conf
echo 'nouveau' | sudo tee -a /etc/modules
卸干净之后再运行
sudo reboot


如遇到安装cuda找不到linux kernels
则：sudo apt-get install linux-headers-generic


安装cudnn

$ sudo tar xvf cudnn-7.0-linux-x64-v4.0-prod.tgz
$ cd cuda/include
$ sudo cp *.h /usr/local/include/
$ cd ../lib64
$ sudo cp lib* /usr/local/lib/
$ cd /usr/local/lib
$ sudo chmod +r libcudnn.so.4.0.4
$ sudo ln -sf libcudnn.so.4.0.4 libcudnn.so.4
$ sudo ln -sf libcudnn.so.4 libcudnn.so
$ sudo ldconfig


安装opencv3.2 
sudo apt-get install build-essential 
sudo apt-get install cmake git pkg-config libgtk2.0-dev libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-opencv python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

$ cd opencv-3.2.0
$ mkdir build
$ cd build
$ cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_CUDA=OFF ..
$ make -j4
$ sudo make install

ippicv_linux_20151201.tgz文件替换

gcc/g++版本降级（一定是5版本）

sudo apt-get install gcc-5
gcc --version
ls /usr/bin/gcc*
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 100
sudo update-alternatives --config gcc

sudo apt-get install g++-5
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 100
sudo update-alternatives --config g++


安装caffe
1.配置文件:
# cuDNN acceleration switch (uncomment to build with cuDNN).
 USE_CUDNN := 1
# Uncomment if you're using OpenCV 3
 OPENCV_VERSION := 3
# CUDA directory contains bin/ and lib/ directories that we need.
CUDA_DIR := /usr/local/cuda-8.0
# Uncomment to support layers written in Python (will link against Python libs)
 WITH_PYTHON_LAYER := 1

# Whatever else you find you need goes here.
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial

打开makefile文件，做如下修改：

NVCCFLAGS +=-ccbin=$(CXX) -Xcompiler-fPIC $(COMMON_FLAGS)

替换为：

NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)


LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs


ifeq ($(USE_OPENCV), 1)
	LIBRARIES += opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs

2. 依赖包:

sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install libhdf5-serial-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev

进入里面的PYTHON文件夹,然后输入
sudo apt install python-pip
for req in $(cat requirements.txt); do pip install $req; done

sudo make all
sudo make pycaffe

3. 环境变量设置
sudo gedit ~/.bashrc
export PYTHONPATH=/home/twj/caffe/python:$PYTHONPATH
source ~/.bashrc

安装tensorflow-gpu
pip install tensorflow-gpu


安装teamvieawer

第一步: 从官网下载ubuntu的deb安装包
第二步: 执行命令sudo dpkg --add-architecture i386 
sudo apt-get update
sudo apt-get -f install
sudo dpkg -i teamviewer_12.0.85001_i386.deb


安装ubuntu chrome

sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
sudo apt-get update
sudo apt-get install google-chrome-stable

安装ubuntu spyder
sudo apt install spyder
pip install qtconsole
pip install nbconvert

