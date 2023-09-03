# bbr-v3-pkg


For English, please see [README.EN.md](README.EN.md).

编译支持 bbr-v3 的 linux 内核为 deb/rpm 格式。

## 安装

对于 Debian/Ubuntu 系统，下载 `linux-headers-*.deb` 和 `linux-image-*.deb` 文件，使用 `dpkg -i` 或 `apt install` 进行安装。

对于基于 RPM 的系统（如 Fedora，CentOS，RHEL），请下载 `kernel-headers-*.rpm` 和 `kernel-*.rpm` 文件。

如果需要构建内核模块，还需要下载 `kernel-devel-*.rpm` 文件。

使用 rpm -i 或 dnf install 进行安装。


## 配置

### 启用 bbrv3

```bash
# 如果想启用 bbrv3，流控算法应设置为 `bbr`，如果想使用早期版本的 bbr，流控算法应设置为 `bbr1`。
echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

# 查看当前的TCP流控算法
sysctl net.ipv4.tcp_congestion_control

# 查看可用的TCP流控算法，以module形式被编译的算法将不会显示
sysctl net.ipv4.tcp_available_congestion_control

# 检查内核安装
uname -a

# 查看tcp_bbr模块信息：
modinfo tcp_bbr

# 检查BBR模块是否启动成功：
lsmod

```

## linux 内核 make help:
```bash
make clean            #删除编译中间文件，但是保留配置
make mrproper         #删除包括配置文件的所有构建文件
make distclean        #执行mrproper所做的一切，并删除备份文件

make menuconfig       #文本图形方式配置内核
make oldconfig        #基于当前的.config文件提示更新内核
make defconfig        #生成默认的内核配置
make allmodconfig     #所有的可选的选项构建成模块
make allyesconfig     #生成全部选择是内核配置
make noconfig         #生成全部选择否的内核配置

make all              #构建所有目标
make bzImage          #构建内核映像
make modules          #构建所有驱动
make dir/             #构建指定目录
make dir/file.[s|o|i] #构建指定文件
make dir/file.ko      #构建指定驱动

make install          #安装内核
make modules_install  #安装驱动

make xmldocs          #生成xml文档
make pdfdocs          #生成pdf文档
maek htmldocs         #生成html文档
```

## 安装编译所需的依赖包和一些常用的软件包

```bash
sudo apt -y update
sudo apt -y install build-essential libncurses-dev libssl-dev libelf-dev bison bc flex rsync debhelper dwarves git
```

## 获取内核源代码 TCP BBR v3 

```bash
git clone -o google-bbr -b v3  https://github.com/google/bbr.git
cd bbr/
```

## 复制系统默认的内核配置文件：

```bash
cp /boot/config-$(uname -r) .config
```

## 编译并创建deb软件包：

```bash
make bindeb-pkg -j$(nproc)
```

## 编译安装内核模块

```bash
make modules_install
```

## 安装内核核心文件

```bash
make install
```

## 内核编译命令
编译内核生成 centos rpm 或 ubuntu deb 包

```bash
make rpm          #生成带源码的RPM包
make rpm-pkg      #生成带源码的RPM包,同上
make binrpm-pkg   #生成包含内核和驱动的RMP包
make deb-pkg      #生成带源码的debian包
make bindeb-pkg   #生成包含内核和驱动的debian包
```





