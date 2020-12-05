# Linux系统移植

+ Bootloader
+ Linux内核
+ 文件系统

## Uboot

### 常用命令

##### printenv查看环境变量

##### setenv修改环境变量

##### 修改环境变量：setenv 环境变量名环境变量值

##### 删除环境变量：setenv 环境变量名

##### saveenv保存环境变量

##### boot启动内核

##### reset重新启动

##### bootm 引导uImage镜像启动

##### bootz 从内存启动Linux镜像和设备

##### ping 测试与主机的网络连接

##### loadb 通过串口Kermit协议下载二进制数据到内存制定位置

##### loady 通过串口线下载文件到内存

##### go 跳转到内存地址处，运行程序

##### env default -a 重置环境变量

##### run 执行环境变量中的命令

##### ls \<interface\> [\<dev[:part]\> [directory]]：uboot 下查看文件系统结构

#### eMMC命令

##### mmc info

##### mmc list

##### mmc read addr blk# cnt

##### mmc write addr blk# cnt

##### mmc erase blk# cnt

> addr 为内存地址，blk#是mmc 的块号，cnt 是设备块的个数，块的单位是512 字节。

#### 网络相关命令

设置MAC地址: setenv ethaddr 73:70:F4:E6:36:EA

设置开发板IP 地址：setenv ipaddr 192.168.1.120

设置服务器的IP 地址(Ubuntu)：setenv serverip 192.168.1.12

设置网关：setenv gatewayip 192.168.1.1

设置子网掩码：setenv netmask 255.255.255.0

> 设置完成后saveenv保存