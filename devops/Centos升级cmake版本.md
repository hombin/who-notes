# Centos 升级 CMake 版本



#### 版本信息

```shell
# 查看 系统信息
cat /etc/*release
# 查看 cmake 信息（Centos）
yum info cmake

#Available Packages
#Name        : cmake
#Arch        : x86_64
#Version     : 2.8.12.2
#Release     : 2.el7
#Size        : 7.1 M
#Repo        : os/7/x86_64
#Summary     : Cross-platform make system
#URL         : http://www.cmake.org
#License     : BSD and MIT and zlib
#Description : CMake is used to control the software compilation process using simple
#            : platform and compiler independent configuration files. CMake generates
#            : native makefiles and workspaces that can be used in the compiler
#            : environment of your choice. CMake is quite sophisticated: it is possible
#            : to support complex environments requiring system configuration, preprocessor
#            : generation, code generation, and template instantiation.
```



#### 升级步骤

```shell
# 创建编译目录
mkdir -p /data/tools/cmake && cd /data/tools/cmake
# 下载 cmake 升级版本 的 tar 包
wget https://cmake.org/files/v3.6/cmake-3.6.2.tar.gz
tar -zxvf cmake-3.6.2.tar.gz
cd cmake-3.6.2
```

```shell
# 编译
sudo ./bootstrap
sudo make
sudo make install
# 验证
/usr/local/bin/cmake --version
```

```shell
# 移除装机带的 cmake
yum remove cmake -y
# 创建系统目录 软链接
ln -s /usr/local/bin/cmake /usr/bin/
# 验证
cmake --version
```




