# WebRTCNote

## WebRTC安卓编译
    只有一台Windows的电脑。三个方案，完成WebRTC的Android平台编译。

---
## 必要的准备
    一个良好的梯子！！！这点很重要！很重要！很重要！大多过程中的问题都源自于网络问题。
* 科学上网
    - blinkload  
        个人的推广链接: https://blinkload.to/aff/UXiq
    - clash for windows  
        github地址: https://github.com/Fndroid/clash_for_windows_pkg/releases

---
### 方案一：适用于Linux的Windows子系统（推荐）

* 启用子系统  
    -控制面板  
    -程序和功能  
    -启用或关闭Windows功能  
    -适用于Linux的Windows子系统  
* UWP版Ubuntu  
    -Microsoft Store  
    -Ubuntu
* Windows Terminal  
    -Microsoft Store  
    -Windows Terminal
* 创建工作目录  
    cd /mnt/  
    cd /e/（随便哪个盘，体验Windows与Liunx的合体，无缝访问）  
    mkdir work

---
### 方案二：Docker中的Ubuntu

* docker  
    - 下载  
    官网地址: https://www.docker.com/
    - 设置docker代理  
    Settings-Resources-PROXIES
    - 设置docker容器代理  
    默认地址为: C:\\Users\\Administrator\\.docker  
    编辑config.json  
    加入代理内容
    ```json
    "proxies": {
        "default": {
            "httpProxy": "http://127.0.0.1:7890",
            "httpsProxy": "http://127.0.0.1:7890",
            "noProxy": ""
        }
    },
    ```
* ubuntu
    - 搜索并拉取  
    ```
    docker search ubuntu
    docker pull ubuntu
    ```
    - 启动并映射
    ```
    docker run -i -t -v D:\xxx:/work ubuntu /bin/bash
    ```
---
### 方案三：虚拟机安装
* 下载VMWare  
    https://www.vmware.com/
* 下载Ubuntu镜像  
    https://ubuntu.com/download/desktop
* 安装启动  
    balabala...
* 建立映射  
    balabala...
---
## 编译
* 编译准备
    - 工具准备
    ```
    apt-get update
    apt-get install git
    apt-get install curl
    apt-get install wget
    apt-get install python
    ```
    - 文件准备
    ```
    cd /work
    export WORKSPACE=`pwd`
    git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
    export PATH=$PATH:$WORKSPACE/depot_tools
    cd $WORKSPACE
    mkdir webrtc
    cd $WORKSPACE/webrtc
    fetch --nohooks webrtc_android
    gclient sync
    ```
* 编译
    ```
    cd $WORKSPACE/webrtc/src
    ./build/install-build-deps.sh
    ./build/install-build-deps-android.sh
    source ./build/android/envsetup.sh
    gn gen android/Release "--args=target_os=\"android\" target_cpu=\"arm64\" is_debug=false rtc_include_tests=false rtc_use_h264=true"
    ninja -C android/Release
    ```