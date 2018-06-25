---
layout: post
title: Linux 如何自定义分辨率
tags: 操作系统
---
![ratio]({{site.baseurl}}/images/posts/ratio.jpeg)

## The two ways to add special resolution ratio

### Add options of the `resolution ratio`

- config `~/.profile`, append the contents like that:

```
cvt 1280 720

xrandr --newmode "1280x720_60.00" 74.50 1280 1344 1472 1664 720 723 728 748 -hsync +vsync

xrandr --addmode LVDS1 "1280x720_60.00"
```
- then execute the command like that:

```
#xrandr --output LVDS1 --mode 1280x720_60.00
```
---

### Update or Create `/etc/X11/xorg.conf`, and append the contents like that:

- 1.在终端输入：

```
#cvt 1280 720

# 1280x720 59.86 Hz (CVT 0.92M9) hsync: 44.77 kHz; pclk: 74.50 MHz

Modeline "1280x720_60.00"   74.50  1280 1344 1472 1664  720 723 728 748 -hsync +vsync
```
- 2.在终端输入:

```
#xrandr

    Screen 0: minimum 320 x 200, current 1280 x 720, maximum 32767 x 32767

    LVDS1 connected primary 1280x720+0+0 (normal left inverted right x axis y axis) 309mm x 174mm

	1366x768       60.0 +

	1360x768       59.8     60.0

	1280x720       59.9*

	1024x768       60.0

	800x600        60.3     56.2

	640x480        59.9

	1280x720_60.00   59.9

	VGA1 disconnected (normal left inverted right x axis y axis)

	HDMI1 disconnected (normal left inverted right x axis y axis)

	DP1 disconnected (normal left inverted right x axis y axis)

	VIRTUAL1 disconnected (normal left inverted right x axis y axis)
```

- 3.在终端输入:

```
# sudo gedit /etc/X11/xorg.conf

- 打开xorg.conf文件，录入以下信息:

	Section "Monitor"

	Identifier "Configured Monitor"

	Modeline "1280x720_60.00" 74.50 1280 1344 1472 1664 720 723 728 748 -hsync +vsync //来自步骤1

	Option "PreferredMode" "1280x720_60.00" //来自步骤1

	EndSection


	Section "Screen"

	Identifier "Default Screen"

	Monitor "Configured Monitor"

	Device "Configured Video Device"

	EndSection

	Section "Device"

	Identifier "Configured Video Device"

	EndSection
```
保存，重启，查看效果，没有效果在可以在“显示(display)”面板选择已添加的分辨率，保存即可。


