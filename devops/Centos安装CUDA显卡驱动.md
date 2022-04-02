# CUDA 显卡驱动安装



#### CUDA

1. 系统信息

    ```shell
    cat /etc/*release
    # 只看系统
    cat /etc/redhat-release
    # CentOS Linux release 7.6.1810 (Core)
    ```

2. 显卡信息

    ```shell
    # 获取设备信息
    lspci -vnn
    
    ##########
    00:0c.0 3D controller [0302]: NVIDIA Corporation TU104GL [Tesla T4] [10de:1eb8] (rev a1)
            Subsystem: NVIDIA Corporation Device [10de:12a2]
            Physical Slot: 12
            Flags: bus master, fast devsel, latency 0, IRQ 11
            Memory at fd000000 (32-bit, non-prefetchable) [size=16M]
            Memory at e0000000 (64-bit, prefetchable) [size=256M]
            Memory at f2000000 (64-bit, prefetchable) [size=32M]
            Capabilities: [60] Power Management version 3
            Capabilities: [68] #00 [0080]
            Capabilities: [78] Express Endpoint, MSI 00
            Capabilities: [c8] MSI-X: Enable- Count=6 Masked-
            Capabilities: [d4] Vendor Specific Information: Len=08 <?>
            Kernel driver in use: nvidia
            Kernel modules: nouveau, nvidia_drm, nvidia
    ```

3. 部分 云服务商GPU机器 自带驱动 可查看N卡信息

    ```shell
    nvidia-smi
    ##########
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 510.47.03    Driver Version: 510.47.03    CUDA Version: 11.6     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  Tesla T4            Off  | 00000000:00:0C.0 Off |                    0 |
    | N/A   42C    P0    27W /  70W |      0MiB / 15360MiB |     10%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
                                                                                   
    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    ```

4. 没有 N卡驱动 则需要先安装（未在不同环境验证）

    ```shell
    yum install kernel-devel gcc -y
    # 检查内核版本
    ls /boot | grep vmlinu
    rpm -aq | grep kernel-devel
    
    # 查看 系统自带 nouveau，有则需要屏蔽
    lsmod | grep nouveau
    ```

    ```shell
    # 屏蔽步骤
    # 修改dist-blacklist.conf文件：
    vim /lib/modprobe.d/dist-blacklist.conf
    
    #将nvidiafb注释掉:
    #blacklist nvidiafb 
    #然后添加以下语句：
    #blacklist nouveau
    #options nouveau modeset=0
    
    # 屏蔽后重建initramfs image步骤
    mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak
    dracut /boot/initramfs-$(uname -r).img $(uname -r)
    systemctl set-default multi-user.target
    reboot
    ```

    ```shell
    # 在NVIDIA官网下载显卡对应驱动
    https://www.nvidia.cn/Download/index.aspx?lang=cn
    # 修改执行权限
    chmod +x NVIDIA-Linux-x86_64-440.64.run
    # 执行
    ./NVIDIA-Linux-x86_64-440.64.run
    # 如果报错 unable to find the kernel source tree for the currently running kernel.........
    # 使用下面命令安装，3.10.0-1062.18.1.el7.x86_64需要改成自己的目录
    ./NVIDIA-Linux-x86_64-440.64.run --kernel-source-path=/usr/src/kernels/3.10.0-1062.18.1.el7.x86_64 -k $(uname -r)
    
    # 安装成功，使用下面命令可查看 驱动详情
    nvidia-smi
    ```

5. 根据 驱动版本 安装 CUDA

    ```shell
    # 检查 是否已安装 cuda
    ls /usr/local | grep cuda
    
    # 驱动 Driver Version: 510.47.03
    # 在官网根据 显卡型号 + 驱动 + 系统版本 选择 cuda
    https://developer.nvidia.com/cuda-downloads
    # 下载 rpm 包
    wget https://developer.download.nvidia.com/compute/cuda/11.6.2/local_installers/cuda-repo-rhel7-11-6-local-11.6.2_510.47.03-1.x86_64.rpm
    # 
    sudo rpm -i cuda-repo-rhel7-11-6-local-11.6.2_510.47.03-1.x86_64.rpm
    sudo yum clean all
    sudo yum -y install nvidia-driver-latest-dkms cuda
    sudo yum -y install cuda-drivers
    ```

6. 驱动重装方法 *

    ```shell
    # 如果是 RPM 或 PPA 安装的 cuda 有可能出现原有 显卡驱动 不可用的情况
    # 需要重新下载安装即可
    nvidia-smi
    # bash: nvidia-smi: commmand not found
    
    # 官网下载 显卡对应驱动程序
    NVIDIA-Linux-x86_64-510.47.03.run
    
    # 卸载
    ./NVIDIA-Linux-x86_64-510.47.03.run --uninstall
    # 安装
    ./NVIDIA-Linux-x86_64-510.47.03.run
    # 验证
    nvidia-smi
    ```

    
