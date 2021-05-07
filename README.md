# Deepin Wine for Ubuntu

Deepin-wine 环境的 Ubuntu 移植版，由 wszqkzqk 的源项目和其 Github 的 Issues 重新制作而来。该仓库主要对新版 Ubuntu 做适配，针对老版本还请移步至源项目。

## 二、软件架构

软件架构说明

### （1）安装教程

#### Ubuntu 20.04.2 LTS

```bash
wget -qO- https://cdn.jsdelivr.net/gh/Nofated095/deepin-wine-ubuntu/master/install20042.sh | bash -e
```

### （2）使用说明

下载并安装所需要的deepin-wine容器 *（建议在终端下使用dpkg -i安装容器，否则容易误报依赖错误）*

可使用deepin发布的最新版容器安装包：

1. [QQ](http://packages.deepin.com/deepin/pool/non-free/d/deepin.com.qq.im/)
2. [TIM](http://packages.deepin.com/deepin/pool/non-free/d/deepin.com.qq.office/)
3. [QQ轻聊版](http://packages.deepin.com/deepin/pool/non-free/d/deepin.com.qq.im.light/)
4. [微信](http://packages.deepin.com/deepin/pool/non-free/d/deepin.com.wechat/) 如果出现依赖错误，请下载[这个版本](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.com.wechat_2.6.8.65deepin0_i386.deb)
5. [百度网盘](http://packages.deepin.com/deepin/pool/non-free/d/deepin.com.baidu.pan/)

[其它deepin-wine容器](http://packages.deepin.com/deepin/pool/non-free/d/)

#### 若版本不兼容，可选择下载安装以下旧版包文件：

1. [QQ](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.com.qq.im_9.1.8deepin0_i386.deb)
2. [TIM](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.com.qq.office_2.0.0deepin4_i386.deb)
3. [QQ轻聊版](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.com.qq.im.light_7.9.14308deepin8_i386.deb)
4. [微信](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.com.wechat_2.6.8.65deepin0_i386.deb)
5. [Foxmail](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.com.foxmail_7.2deepin3_i386.deb)
6. [百度网盘](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.com.baidu.pan_5.7.3deepin0_i386.deb)
7. [360压缩](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.cn.360.yasuo_4.0.0.1060deepin3_i386.deb)
8. [WinRAR](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.cn.com.winrar_5.3.0deepin2_i386.deb)
9. [迅雷极速版](https://gitee.com/wszqkzqk/deepin-wine-containers-for-ubuntu/raw/master/deepin.com.thunderspeed_7.10.35.366deepin18_i386.deb)

### （3）关于托盘

Ubuntu 18.04 下（Gnome 桌面）：

安装 Gnome Shell 插件：[TopIcons Plus](https://extensions.gnome.org/extension/1031/topicons/)

### （4）TIM 的可选操作--Flash 的安装方法
*本操作无必要性
1. 下载需要的 install_flash_player_ppapi.exe 
2. 将下载的安装文件放入 ~/.deepinwine/Deepin-TIM/drive_c 下，即 TIM 所在 Wine C 盘根目录
3. 打开一个 Terminal ，执行：
```
WINEPREFIX=~/.deepinwine/Deepin-TIM deepin-wine "c:\\install_flash_player_ppapi.exe"
```
然后按提示进行安装、重启 TIM 即可。

### （5）手动更改配置（winecfg）
执行 `WINEPREFIX=~/.deepinwine/容器名称 deepin-wine winecfg` 即可，也可以用此方法来调整缩放问题

### （6）解决系统非中文语言环境时软件无法设置为中文
在/opt/deepinwine/tools/run.sh 中将 WINE_CMD 那一行修改为 WINE_CMD="LC_ALL=zh_CN.UTF-8 deepin-wine"

### （7）卸载方法

* 执行uninstall.sh即可

### （8）微信更新问题
* 如果出现微信提示更新问题执行这一条语句即可
```
 wget -qO- https://deepin-wine.i-m.dev/setup.sh | sudo sh
```

### （9）TIM提示更新

* 在[TIM官网](https://office.qq.com/download.html)下载TIM 的 Windows 最新版本（exe）
```bash
env WINEPREFIX=~/.deepinwine/Deepin-TIM/ deepin-wine %name%.exe
```

### （10）[wine 应用程序全局快捷键无效的解决方案](https://blog.diqigan.cn/posts/wine-global-hotkey-problem.html) 


#### 1. 安装 xdotool

直接在命令行运行以下命令即可: 

```shell
sudo apt install --no-install-recommends xdotool
```

#### 2. 编写 xdotool 脚本

*思路: Wine 应用在后台无法接收到快捷键状态, 此时借助 xdotool 向 Wine 应用发送模拟按键信息即可. * 

在合适的位置新建一个脚本文件 "open_wechat.sh", 写入以下内容: 

```shell
#!/bin/sh
#在当前运行的应用中找到名为WeChat.exe的应用程序，并向它发送按键事件"ctrl+alt+W"
#WeChat的可执行文件名为WeChat.exe，如果是其它应用程序就修改成其它应用程序的可执行文件名, 应用名称大小写敏感, 一个字母都不能错!
xdotool key --window $(xdotool search --limit 1 --all --pid $(pgrep WeChat.exe)) "ctrl+alt+W"
```

赋予脚本可执行权限: 

```shell
chmod +x open_wechat.sh
```

如果此时你的微信正好运行在后台, 执行这个脚本就可以把它召唤到前台. 如果没有, 请检查脚本是否有错误. 

#### 3. 设置快捷键

图形界面依次打开 "设置" -> "设备" -> "键盘", 点击列表最底部的 "+" 号添加自定义快捷键. 

![快捷键设置](https://images.gitee.com/uploads/images/2020/0117/075141_4d17fab4_1442530.png)

* 名称随便, 填写 "打开微信" 即可; 
* 命令填写刚才编写的脚本的**全路径**;
* 快捷键设置自己想用的快捷键即可, 建议于应用内部快捷键相同; 
* 最后点击"添加"即可. 

#### 4. 验证

到这里已经设置成功了, 打开微信, 切换到后台, 然后按下刚才设置的快捷键就能召唤应用至前台. 如果不能, 请检查自己前面的设置是否有误. 

### （11）问题记录及解决方案

#### 1. 微信无法发送图片
```shell
sudo apt-get install libjpeg62:i386
```