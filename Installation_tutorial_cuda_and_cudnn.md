一、安装cuda

1、下载cuda

网址：https://developer.nvidia.com/cuda-toolkit-archive

按照版本选择

ubuntu选择runfile（local）比较方便

```linux
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.runsudo sh cuda_10.2.89_440.33.01_linux.run
```

以下仅为cuda10.2安装说明，不同版本会略有不同！！！

注意：

```
#..一堆协议说明...
#直接按q退出协议说明.
accept/decline/quit: accept  #接受协议

Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 410.48? 
y)es/(n)o/(q)uit: n  #是否显卡驱动包，由于已经安装显卡驱动，选择n

Install the CUDA 10.0 Toolkit?
(y)es/(n)o/(q)uit: y #是否安装工具包，选择y

Enter Toolkit Location

[ default is /usr/local/cuda-10.0 ]: #工具包安装地址，默认回车即可

Do you want to install a symbolic link at /usr/local/cuda?
(y)es/(n)o/(q)uit: n #添加链接**注意这个连接，如果之前安装过另一个版本的cuda，除非你确定想要用这个新版本的cuda，否则这里就建议选no，因为指定该链接后会将cuda指向这个新的版本**

Install the CUDA 10.0 Samples?
(y)es/(n)o/(q)uit: n #不安装样例
```

安装完，输入nvcc -V若无输出，记得修改~/.bashrc

打开~/.bashrc指令：

```
sudo gedit ~/.bashrc
```

~/.bashrc中在最后一行开始添加

```linux
# cuda
export PATH=/usr/local/cuda/bin:$PATH  
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=/usr/local/cuda
```

更新～/.bashrc指令：

```
sudo source ～/.bashrc
```

二、cudnn安装

下载cudnn文件需要nvidia帐号。

1、下载https://developer.nvidia.com/rdp/cudnn-archive，按照对应系统下载cuDNN Library压缩包。

2、进入到下载文件夹，并右键打开终端，解压cudnn文件：

```
tar -zxvf cudnn-10.0-linux-x64-v7.4.2.24.tgz
```

3、进入到cudnn文件夹复制链接库到cuda目录下，建议不要直接复制到cuda，而是复制到制定版本的cuda-10.0中去：

```linux
cd cudnn-10.0-linux-x64-v7.4.2.24
sudo cp cuda/include/cudnn* /usr/local/cuda-10.0/include/ 
sudo cp cuda/lib64/libcudnn* /usr/local/cuda-10.0/lib64/ 
sudo chmod a+r /usr/local/cuda-10.0/include/cudnn* /usr/local/cuda-10.0/lib64/libcudnn*
```

4、建立cudnn软链接

```
cd /usr/local/cuda-10.0/lib64/
sudo ln -sf libcudnn.so.7.4.2 libcudnn.so.7
source ~/.bashrc
```

注意修改版本号

5、查看cudnn版本

cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2

若没输出就换下一个

cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2

6、更新cudnn版本

得先删除旧版本

```
sudo rm -rf /usr/local/cuda/include/cudnn.h
sudo rm -rf /usr/local/cuda/lib64/libcudnn*
```

安装新版本

```
sudo cp include/* /usr/local/cuda/include/
sudo cp lib64/lib* /usr/local/cuda/lib64/
```

建立软连接

```
cd /usr/local/cuda/lib64/
sudo chmod +r libcudnn.so.7.3.1
sudo ln -sf libcudnn.so.7.3.1 libcudnn.so.7
sudo ln -sf libcudnn.so.7 libcudnn.so   
sudo ldconfig  
```

注意版本号

建完验证下

```
sudo ldconfig
```

注cudnn8.0以上版本，可能有如下报错

```
/sbin/ldconfig.real: /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8 is not a symbolic link

/sbin/ldconfig.real: /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8 is not a symbolic link

/sbin/ldconfig.real: /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8 is not a symbolic link

/sbin/ldconfig.real: /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8 is not a symbolic link

/sbin/ldconfig.real: /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8 is not a symbolic link

/sbin/ldconfig.real: /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_train.so.8 is not a symbolic link
```

那就都新建立下软连接

```
sudo ln -sf  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8.0.2  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8

sudo ln -sf  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8.0.2  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8

sudo ln -sf  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8.0.2  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8

sudo ln -sf  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8.0.2  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8

sudo ln -sf  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8.0.2  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8

sudo ln -sf  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_train.so.8.0.2  /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_train.so.8
```

三、cuda版本切换

1、删除原版本的cuda软连接

```
sudo rm -rf /usr/local/cuda
```

2、建立新的指向cuda-10.0的软连接

```
sudo ln -s /usr/local/cuda-10.0 /usr/local/cuda

sudo ln -s /usr/local/cuda-10.2 /usr/local/cuda
```
