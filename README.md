# 适用于 IPQ系列设备的 OpenWrt 源码仓库

## 注意

1. **不要用 root 用户进行编译**
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1 密码 password

## 编译命令

1. 首先装好 Linux 系统， Ubuntu 24.04 LTS

2. 安装编译依赖

   ```bash
   sudo apt update -y
   sudo apt full-upgrade -y
   sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
   bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
   git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
   libmpc-dev libmpfr-dev libreadline-dev libssl-dev libtool lrzsz \
   mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip libpython3-dev qemu-utils \
   rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
   ```

3. 下载源代码，更新 feeds 并选择配置

   ```bash
   git clone -b main --single-branch https://github.com/darkrain88/openwrt-6.x.git
   cd openwrt-6.x
   ./scripts/feeds update -a && ./scripts/feeds install -a
   make menuconfig
   ```

4. 下载 dl 库，编译固件
（-j 后面是线程数，为便于排除错误推荐用单线程）

   ```bash
   make download -j8
   make -j1 V=s
   ```

5. 二次编译：

   ```bash
   cd openwrt-6.x
   git fetch && git reset --hard origin/main
   ./scripts/feeds update -a && ./scripts/feeds install -a
   make defconfig
   make V=s -j$(nproc)
   ```

6. 如果需要重新配置：

   ```bash
   rm -rf .config
   make menuconfig
   make V=s -j$(nproc)
   ```

7. 编译完成后输出路径：bin/targets
8. 
8 一些配置

# sed -i 's/192.168.1.1/10.0.0.1/g' package/base-files/files/bin/config_generate

# 更改默认 Shell 为 zsh
# sed -i 's/\/bin\/ash/\/usr\/bin\/zsh/g' package/base-files/files/etc/passwd

# TTYD 免登录
# sed -i 's|/bin/login|/bin/login -f root|g' feeds/packages/utils/ttyd/files/ttyd.config

#  socat 支持 fw3 and fw4

git clone https://github.com/chenmozhijin/luci-app-socat  feeds/luci/applications/luci-app-socat

# zerotier
git clone --depth=1 https://github.com/zhengmz/luci-app-zerotier.git feeds/luci/applications/luci-app-zerotier

# ddnsgo
git clone https://github.com/sirpdboy/luci-app-ddns-go.git feeds/luci/applications/ddns-go

# sms-tool

git clone https://github.com/4IceG/luci-app-sms-tool.git  feeds/luci/applications/luci-app-sms-tool

git clone https://github.com/darkrain88/luci-app-sms-tool

git clone --depth=1 https://github.com/xiaorouji/openwrt-passwall-packages package/openwrt-passwall
git clone --depth=1 https://github.com/xiaorouji/openwrt-passwall package/luci-app-passwall
git clone --depth=1 https://github.com/xiaorouji/openwrt-passwall2 package/luci-app-passwall2
git clone --depth=1 https://github.com/kongfl888/luci-app-adguardhome package/luci-app-adguardhome


# MosDNS
git clone --depth=1 https://github.com/sbwml/luci-app-mosdns feeds/luci/applications/luci-app-mosdns

# Adguardhome
git clone --depth=1 https://github.com/kongfl888/luci-app-adguardhome feeds/luci/applications/luci-app-adguardhome
# Alist
git clone --depth=1 https://github.com/sbwml/luci-app-alist package/luci-app-alist


