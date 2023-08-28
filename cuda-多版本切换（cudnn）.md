**1.查看自己已经安装的cuda版本**

```bash
cd /usr/local
```

![image-20230828140808121](/home/peter/.config/Typora/typora-user-images/image-20230828140808121.png)

**2.查看当前版本**

```bash
sata cuda
```

![image-20230828141018685](/home/peter/.config/Typora/typora-user-images/image-20230828141018685.png)

目前软链接指向cuda11.8

**3.下载其他版本cuda**

进入cuda官网下面例子是下载cuda11.7+cudnn8.5

https://developer.nvidia.com/cuda-toolkit-archive 

![image-20230828141423954](/home/peter/.config/Typora/typora-user-images/image-20230828141423954.png)

```bash
wget https://developer.download.nvidia.com/compute/cuda/11.7.0/local_installers/cuda_11.7.0_515.43.04_linux.run
sudo sh cuda_11.7.0_515.43.04_linux.run
```

**4.安装CUDA的操作步骤**

选择需要安装的项，按A进入 CUDA Toolkit 11.7。全部取消。

![image-20230828142851162](/home/peter/.config/Typora/typora-user-images/image-20230828142851162.png)

![image-20230828142826785](/home/peter/.config/Typora/typora-user-images/image-20230828142826785.png)



接下来可以看到cuda11.7和11.8，但是还是指向cuda11.8。

![image-20230828143227791](/home/peter/.config/Typora/typora-user-images/image-20230828143227791.png)

```bash
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
```

**5.安装cudnn**

https://developer.nvidia.com/rdp/cudnn-archive

![image-20230828144248711](/home/peter/.config/Typora/typora-user-images/image-20230828144248711.png)

```bash
tar -xf cudnn-linux-x86_64-8.5.0.96_cuda11-archive.tar.xz 
```



```bash
sudo cp include/cudnn*.h /usr/local/cuda-11.7/include
sudo cp lib/libcudnn* /usr/local/cuda-11.7/lib64
sudo chmod a+r /usr/local/cuda-11.7/include/cudnn*.h  /usr/local/cuda-11.7/lib64/libcudnn*
```



**6.指向你需要的cuda版本**

删除软链接

```bash
sudo rm -rf /usr/local/cuda
```

重新指向你需要的CUDA版本

```bash
sudo ln -s /usr/local/cuda-11.7 /usr/local/cuda
```

![image-20230828145703525](/home/peter/.config/Typora/typora-user-images/image-20230828145703525.png)

查看cudnn版本 

```bash
cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```

![image-20230828145922343](/home/peter/.config/Typora/typora-user-images/image-20230828145922343.png)