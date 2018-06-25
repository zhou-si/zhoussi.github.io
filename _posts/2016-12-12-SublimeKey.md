---
layout: post
title: Sublime 键位教程
tags: 工具
---

![sublime]({{site.baseurl}}/images/posts/sublime.jpeg)

> 建议，看一遍即可，在使用的过程中，按需检索`ctrl+f`

### 光标移动.定位

```
U D L R
↑ ↓ ← →

Ctrl+← 向左`单位性`地移动光标，快速移动光标
Ctrl+→ 向右`单位性`地移动光标，快速移动光标

Tab       向右缩进
Shift+Tab 向左缩进

Ctrl+LB  按下的同时鼠标左键选择多处定位

Ctrl+G  跳转指定行
```

### 光标特征定位

```
Ctrl+M 光标移动至括号内结束或开始的位置
Ctrl+Shift+L 先选中多行，再按下快捷键，会在每行行尾插入光标
```

### 多行操作
```
Ctrl+Alt+↑ 向上添加多行光标，可同时编辑多行
Ctrl+Alt+↓ 向下添加多行光标，可同时编辑多行

shift+↑ 向上选中多行
shift+↓ 向下选中多行
Shift+← 向左选中文本
Shift+→ 向右选中文本

Ctrl+[ 向左缩进,对整行有效
Ctrl+] 向右缩进,对整行有效

Ctrl+Shift+← 向左单位性地选中文本
Ctrl+Shift+→ 向右单位性地选中文本

Ctrl+T 左右字母互换
Ctrl+Shift+↑ 将光标所在行整行向上
Ctrl+Shift+↓ 将光标所在行整行向下

Ctrl+Enter        当前行后插入下一行
Ctrl+Shift+Enter  当前行上插入一行

Ctrl+G  跳转指定行
```



### 选择

```
Ctrl+D  选中光标所占的单词(连续的中文会被认为是一个单词单位)
Alt+F3  选中后,按下快捷键,可一次性选择全部相同文本进行同时编辑(linux下有冲突)

Ctrl+L          选中整行,连续会继续选择下一行
Ctrl+Shift+M    选中当前括号内容，重复可选着括号本身
Ctrl+Shift+Alt  光标所在处容器,继续选择父容器

Ctrl+Shift+`  选择当前标签前后，修改标签用的

alt+shift+w   用标签包裹行或选中项
ctrl+shift+;  移除未闭合的容器元素
```

### 复制 粘贴 删除 格式化
```
Ctrl+C  复制选中文本或整行(连同换行符也会一起复制哦)
Ctrl+D  粘贴复制内容
Ctrl+Shift+V  粘贴并格式化

Ctrl+Shift+D  复制光标所在整行，插入到下一行

Ctrl+x        剪切
Ctrl+Shift+K  删除整行
Ctrl+K+K      从光标处开始删除代码至行尾

Ctrl+J 合并选中的多行代码为一行

Ctrl+K+U 转换大写
Ctrl+K+L 转换小写

```

### 撤销
```
Ctrl+Z 撤销
Ctrl+U 软撤销
Ctrl+Y 恢复撤销
```
### 搜索
```
Ctrl+P  打开搜索框
  - 当前项目中的文件名
  - @和关键字,搜索文件中的函数  Ctrl+R
  - #和关键字,搜索文件中的变量  Ctrl+:
  - :和数字,跳转到文件中指定行  Ctrl+G

Ctrl+Shift+P  打开命令框,调用Subl或插件的功能

Esc           退出光标多行选择,退出搜索框,命令框

Ctrl+F        文件中搜索
Ctrl+Shift+F  多文件中搜索和替换
Ctrl+H        文件中搜索和替换

```

### 标签屏幕
```
Ctrl+Tab  切换窗口标签


Alt+Shift+1 恢复默认-1屏

Alt+Shift+2 左右分屏-2列
Alt+Shift+3 左右分屏-3列
Alt+Shift+4 左右分屏-4列

Alt+Shift+5 等分4屏

Alt+Shift+8 垂直分屏-2屏
Alt+Shift+9 垂直分屏-3屏


Alt+数字：切换打开第N个文件

Ctrl+K+B 开启/关闭侧边栏

F11 全屏模式
Shift+F11 免打扰模式

```

### 注释

```

Ctrl+/ 注释单行
Ctrl+Shift+/ 注释多行
Ctrl+Alt+/：块注释，并Focus到首行，写注释说明用的

```

### **sublime license**

```
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB 
```

条件允许了，请支持正版