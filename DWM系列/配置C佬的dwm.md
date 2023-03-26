# 配置C佬的DWM

[什么是DWM](https://zh.wikipedia.org/wiki/Dwm)

## 仓库地址

[yaocccc/dwm](https://github.com/yaocccc/dwm)

[yaocccc/picom](https://github.com/yaocccc/picom)

[yaocccc/wallpaper](https://github.com/yaocccc/wallpaper)

[yaocccc/scripts](https://github.com/yaocccc/scripts)

[yaocccc/st](https://github.com/yaocccc/st)

## 安装xorg

```
sudo pacman -S xorg-server

sudo pacman -S xorg-xinit xorg-twm xterm xorg-xclock
```

## 安装字体

```
yay -S nerd-fonts-jetbrains-mono
yay -S ttf-material-design-icons
yay -S ttf-joypixels
yay -S wqy-microhei
```

## 克隆代码

```
git clone https://github.com/yaocccc/dwm .dwm
```

`由于国内网络问题，建议中国大陆用户通过ssh克隆`

```
git clone git@github.com:yaocccc/dwm.git .dwm
```

进入源码目录

```
cd .dwm

## 按照yaocccc的README文档：

cp -r DEF/* .

sudo make clean install
```

### 配置环境变量

```
vim ~/.xinitrc
```

加入：

```
export DWM=~/.dwm
exec dwm
```

## 安装相关依赖

```
sudo pacman -S flameshot dunst xss-lock pactl bluez bluez-utils acpi upower feh cron

sudo pacman -S xf86-input-synaptics xdotool btop

yay -S neovim

##下面安装picom也会用到

sudo pacman -S gcc asciidoc meson python ninja

sudo pacman -S libx11 libxext xorgproto xcb-util 

sudo pacman -S pixman libdbus libconfig libgl pcre libev uthash
```

### 克隆scripts

```
git clone git@github.com:yaocccc/scripts.git .scripts
```

### 修改autostart.sh

```
vim ~/.dwm/autostart.sh
```

把 `~/script` 修改为 `~/.scripts`

## 克隆wallpaper

```
git clone git@github.com:yaocccc/wallpaper.git Pictures
```

## 克隆并安装st

```
git clone git@github.com:yaocccc/st.git

cd st && sudo make clean install
```

## 克隆并安装picom

```
git clone git@github.com:yaocccc/picom.git

git submodule update --init --recursive

meson --buildtype=release . build

ninja -C build install
```

## `如果想在tty1中自动执行startx可在你的bash或zsh配置中添加`

```
[ $(tty) = "/dev/tty1" ] && cd ~ && startx
```

## 大功告成！