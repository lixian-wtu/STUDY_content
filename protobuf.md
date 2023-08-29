

**确保protobuf-compiler和protobuf版本一致性**

```bash
sudo apt-get install protobuf-compiler # 查看库版本
```

![image-20230829105314054](/image/image-20230829105314054.png)

**找到对应的protobuf版本编译安装**

https://github.com/protocolbuffers/protobuf/releases

```bash

tar -zxvf protobuf-3.6.1.tar.gz # 解压

sudo apt-get install build-essential # 不装会报错

cd protobuf-3.6.1/ # 进入目录

./configure # 配置安装文件

make -j16 # 编译

make check # 检测编译安装的环境

sudo make install # 安装
```

