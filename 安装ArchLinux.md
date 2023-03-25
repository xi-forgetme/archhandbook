# 安装Arch Linux

## 连接网络

```
ip link  #列出网络接口信息，如不能联网的设备叫wlan0
ip link set wlan0 up #比如无线网卡看到叫 wlan0

# 如果无线网卡出现 Operation not possible due to RF-kill 报错
rfkill unblock wifi

iwctl                           #执行iwctl命令，进入交互式命令行
device list                     #列出设备名，比如无线网卡看到叫 wlan0
station wlan0 scan              #扫描网络
station wlan0 get-networks      #列出网络 比如想连接YOUR-WIRELESS-NAME这个无线
station wlan0 connect YOUR-WIRELESS-NAME #进行连接 输入密码即可
exit                            #退出
```
## 更新系统时钟

```
timedatectl set-ntp true    #将系统时间与网络时间进行同步
timedatectl status          #检查服务状态
```

## 硬盘分区

这里放出来一个比较常用的方案

```
/dev/sdX
    |
    |-sdX1  EFI Filesystem 300MB    FAT32
    |
    |-sdX2  Linux Swap 4GB  Swap
    |
    |-sdX3  Linux Filesystem    ext4
```

首先 `lsblk` 查看一下设备名称

通过 `cfdisk /dev/sdX` 命令分区， 选择GPT

分区结束后查看一下分区情况 `fdisk -l`

然后格式化刚才所创建的分区

```
mkfs.vfat /dev/sdX1

mkswap /dev/sdX2    #这是swap分区

mkfs.ext4 /dev/sdX3
```

## 挂载磁盘

```
mount /dev/sdX3 /mnt

mkdir /mnt/boot

mount /dev/sdX1 /mnt/boot

swapon /dev/sdX2    #启用swap分区
```

## 安装基本系统

```
pacstrap /mnt base linux linux-firmware base-devel nano vim
```

### 生成FSTAB文件

```
genfstab -U /mnt >> /mnt/etc/fstab

cat /mnt/etc/fstab #检查生成的文件
```

## 配置系统

### 切换到安装好的系统

```
arch-chroot /mnt
```

#### 设置时区

```
timedatectl set-timezone Asia/Taipei

hwclock --systohc   #同步硬件时钟
```

#### 设置本地化

```
vim /etc/locale.gen
```

搜索`en_US.UTF-8 UTF-8` 和 `zh_CN.UTF-8 UTF-8`，注释掉前面的 `#`

```
locale-gen  #生成locale

echo 'LANG=en_US.UTF-8' >> /etc/locale.conf #设置默认locale为en_US
```

#### 设置hostname

```
echo 'localhost' >> /etc/hostname
```

#### 设置hosts

```
vim /etc/hosts
```

将以下内容写入

```
127.0.0.1   localhost
::1         localhost
127.0.1.1   localhost.localdomain
```

#### 设置用户密码

修改root密码

```
passwd
```

创建新用户

```
useradd -m -g users -s /bin/bash user   #user为用户名

passwd user #为新用户创建密码
```

把新用户添加到sudoers

```
visudo
```

添加

```
user ALL=(ALL:ALL) ALL
```

Tip: 如果希望新用户可以不用输入密码执行 `sudo` 命令，请添加

```
user ALL=(ALL:ALL) NOPASSWD:ALL
```

#### 安装基础软件包

```
pacman -S networkmanager network-manager-applet dialog wireless_tools os-prober mtools dosfstools ntfs-3g linux-headers reflector git sudo

pacman -S htop git curl wget

pacman -S intel-ucode   #Intel CPU

pacman -S amd-ucode     #AMD CPU
```

### 安装引导程式 `这里以GRUB为例`

```
pacman -S grub efibootmgr
```

`注意：如果您是双系统用户，请安装os-prober`

```
grub-install --efi-directory=/boot --bootloader-id=Arch

grub-mkconfig -o /boot/grub/grub.cfg    #生成grub.cfg
```

### 开启NetworkManager方便开机网络连接

```
systemctl enable NetworkManager
```

## 附加

### 添加archlinuxcn源

```
vim /etc/pacman.conf
```

添加内容

```
[archlinuxcn]
Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch
```

同时取消对`multilib`源的注释

保存退出，同步并安装`archlinuxcn-keyring`

```
pacman -Syu && pacman -S archlinuxcn-keyring
```

安装 `yay`

```
pacman -S yay
```

### 安装VMTools

```
pacman -S open-vm-tools
```

设置开机启动

```
systemctl enable vmtoolsd.service

systemctl enable vmware-vmblock-fuse.service
```

安装`xf86-video-vmware`

```
pacman -S xf86-video-vmware
```

安装`gtkmm`和`gtk2`

```
pacman -S gtkmm gtk2
```

添加模块

```
vim /etc/mkinitcpio.conf
```

`MODULES=()`里加入

```
vsock vmw_vsock_vmci_transport vmw_balloon vmw_vmci vmwgfx
```

执行

```
mkinitcpio -p linux
```

根据官方wiki，安装 `gtkmm3` 以支持拖拽

与宿主机同步时间

```
vmware-toolbox-cmd timesync enable

hwclock --hctosys --localtime
```

## 注意

务必记住一周至少更新一次系统，否则容易滚挂

```
pacman -Syyu
```

清除pacman缓存的软件包

```
pacman -Scc
```

## 现在就可以体验你的新系统了！

退出chroot

```
exit
```

卸载分区

```
umount /mnt/boot

umount /mnt
```

同步磁盘缓存

```
sync
```

重启进入新系统

```
reboot
```