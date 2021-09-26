
App filtering is a parent management plug-in based on OpenWrt, which supports app filtering for games, videos, chats, downloads, etc.

### How to compile application - Using OpenWRT SDK （Compling Log is saved for your reference)

- Step 1 -- get Openwrt-SDK for your router devices

- Step 2 -- git clone src to package/Extra-plugin/OpenAppFilter

- Step 3 -- feeds update && make 

- Step 4 -- SCP ipks to OpenWrt && Install


应用过滤是一款基于OpenWrt的家长管理插件，支持游戏、视频、聊天、下载等app过滤  
### 如何编译应用过滤固件（编译详细过程已保存）
1. 下载自己路由器对应的OpenSDK

2. clone应用过滤源码 

3. 更新feeds并编译

4. 上传ipk并安装

### Instructions for use
1. Make the application filtering device the main route  
2. Turn off software and hardware acceleration, advertising filtering, QOS, multi-WAN and other modules related to nf_conn mark  
3. Turn on application filtering and select the app that needs to be filtered to take effect  
4. Add Block Address to /etc/appfilter/feature.cfg (OpenAppFilter/open-app-filter/files/feature.cfg) 

### 使用说明
1. 将应用过滤设备做主路由  
2. 关闭软硬加速、广告过滤、QOS、多WAN等涉及到nf_conn mark的模块  
3. 开启应用过滤并选择需要过滤的app即可生效  
4. 添加自定义屏蔽地址到 /etc/appfilter/feature.cfg (OpenAppFilter/Open-app-filter/files/feature.cfg)



### Compiling log 编译过程详情

```
################################

# Step 1 -- get Openwrt-SDK for your router devices
admin@debian:~$ wget https://mirror.tuna.tsinghua.edu.cn/openwrt/releases/18.06.8/targets/ XXX /openwrt-sdk-18.06.8-XXX _gcc-7.3.0_mus l.Linux-x86_64.tar.xz
admin@debian:~$ tar -xf openwrt*sdk*tar.xz

admin@debian:~$ ls ./openwrt-sdk-18.06.8-ramips-mt7620_gcc-7.3.0_musl.Linux-x86_64$ 
bin        Config-build.in  dl     feeds.conf.default  LICENSE   package     README.SDK  scripts      target
build_dir  Config.in        feeds  include             Makefile  prepare.mk  rules.mk    staging_dir  tmp

################################
# Step 2 -- git clone src to package/Extra-plugin/OpenAppFilter
# rename folder to openwrt-sdk-Linux-x86_64
admin@debian:~$ cd openwrt*sdk*64
admin@debian:~/openwrt-sdk-Linux-x86_64$ mkdir -p package/Extra-plugin 
admin@debian:~/openwrt-sdk-Linux-x86_64$ cd package/Extra-plugin
admin@debian:~/openwrt-sdk-Linux-x86_64/package/Extra-plugin$ git clone https://github.com/destan19/OpenAppFilter
admin@debian:~/openwrt-sdk-Linux-x86_64/package/Extra-plugin$ cd ../..
admin@debian:~/openwrt-sdk-Linux-x86_64$ tree package/Extra-plugin -L 2
package/Extra-plugin
└── OpenAppFilter
    ├── luci-app-oaf
    ├── oaf
    ├── open-app-filter
    └── README.md

4 directories, 1 file

################################
# Step 3 -- feeds update && make 
admin@debian:~/openwrt-sdk-Linux-x86_64$ ./scripts/feeds update -a && ./scripts/feeds install -a
# (直接编译固件失败)(只编译ipk包成功) make menuconfig and select net/luci-app-oaf; make (build bin FAILED); make pakcages as below 
admin@debian:~/openwrt-sdk-Linux-x86_64$ make package/Extra-plugin/OpenAppFilter/ oaf
make: 'package/Extra-plugin/OpenAppFilter/oaf' is up to date.
admin@debian:~/openwrt-sdk-Linux-x86_64$ make package/Extra-plugin/OpenAppFilter/ open-app-filter
make: 'package/Extra-plugin/OpenAppFilter/open-app-filter' is up to date.
admin@debian:~/openwrt-sdk-Linux-x86_64$ make package/Extra-plugin/OpenAppFilter/ luci-app-oaf
make: 'package/Extra-plugin/OpenAppFilter/luci-app-oaf' is up to date.
#----success build ipk packges as below------
admin@debian:~/openwrt-sdk-Linux-x86_64$ ls -lh bin/packages/mipsel_24kc/base/app filte*ipk
-rw-r--r-- 1 admin admin 6.8K Mon 99 24:00 bin/packages/mipsel_24kc/base/appfilter_3.0-1_mipsel_24kc.ipk
admin@debian:~/openwrt-sdk-Linux-x86_64$ ls -lh bin/packages/mipsel_24kc/base/luc i*oaf*ipk
-rw-r--r-- 1 admin admin 4.1K Mon 99 24:00 bin/packages/mipsel_24kc/base/luci-app-oaf_3.0-1_all.ipk
-rw-r--r-- 1 admin admin 1.6K Mon 99 24:00 bin/packages/mipsel_24kc/base/luci-i18n-oaf-zh-cn_3.0-1_all.ipk
admin@debian:~/openwrt-sdk-Linux-x86_64$ ls -lh bin/targets/ramips/mt7620/package s/kmod-oaf*ipk
-rw-r--r-- 1 admin admin 23K Mon 99 24:00 bin/targets/ramips/mt7620/packages/kmod-oaf_4.14.171+3.0-1_mipsel_24kc.ipk

################################
# Step 4 -- SCP ipks to OpenWrt && Install

#scp *ipk root@192.168.1.1:/tmp
root@192.168.1.1's password:
appfilter_3.0-1_mipsel_24kc.ipk              100% 6959   535.7KB/s   00:00
kmod-oaf_4.14.171+3.0-1_mipsel_24kc.ipk      100%   22KB   1.5MB/s   00:00
luci-app-oaf_3.0-1_all.ipk                   100% 4116   823.8KB/s   00:00
luci-i18n-oaf-zh-cn_3.0-1_all.ipk            100% 1566   391.8KB/s   00:00

#ssh root@openwrt
root@OpenWrt:~# cd /tmp
root@OpenWrt:/tmp# opkg install kmod-oaf*ipk
root@OpenWrt:/tmp# opkg install appfileter*ipk
root@OpenWrt:/tmp# opkg install luci-app-oaf*ipk
root@OpenWrt:/tmp# opkg install luci-iu18n-oaf*ipk

```
