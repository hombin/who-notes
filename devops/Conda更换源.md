# Conda 更换源



1. **2021** 年前的 conda 版本 换源

    ```yaml
    vim ~/.condarc
    
    # 清华源
    channels:
      - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
      - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
      - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
      - defaults
    show_channel_urls: true
    ```

    

2. **2021** 年后的 conda 版本 换源

    ```yaml
    vim ~/.condarc
    
    # 清华源
    channels:
      - defaults
    show_channel_urls: true
    default_channels:
      - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
      - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
      - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
    custom_channels:
      conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
      msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
      bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
      menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
      pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
      pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
      simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
      
    # 阿里源
    channels:
      - defaults
    show_channel_urls: true
    default_channels:
      - https://mirrors.aliyun.com/anaconda/pkgs/main
      - https://mirrors.aliyun.com/anaconda/pkgs/r
      - https://mirrors.aliyun.com/anaconda/pkgs/msys2
    custom_channels:
      conda-forge: https://mirrors.aliyun.com/anaconda/cloud
      msys2: https://mirrors.aliyun.com/anaconda/cloud
      bioconda: https://mirrors.aliyun.com/anaconda/cloud
      menpo: https://mirrors.aliyun.com/anaconda/cloud
      pytorch: https://mirrors.aliyun.com/anaconda/cloud
      pytorch-lts: https://mirrors.aliyun.com/anaconda/cloud
      simpleitk: https://mirrors.aliyun.com/anaconda/cloud
    ```

    
