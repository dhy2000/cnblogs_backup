# Ubuntu 便携盘 安装笔记

记录一些系统安装和配置过程中踩过的坑。版本 Ubuntu 20.04 LTS 。

## 系统安装

使用固态移动硬盘或固态 U 盘 (例如 aigo U393) 安装便携的 Ubuntu 系统(可以在其他机器上启动，且无需担心误操作损坏硬盘的数据)。

从 Ubuntu Server 镜像而不是 Desktop 镜像安装，Server 镜像体积小，安装很快。

Tip: 可以用 Virtualbox 安装系统，将准备装系统的 U 盘分配给虚拟机，即可在只有一块盘的环境下安装系统，不用担心引导安装错。另外，刚安装好后可以借助主机的网络完成软件安装。

将默认的 apt 源替换为清华源: `https://mirrors.tuna.tsinghua.edu.cn`

Virtualbox 注意事项:

1. 需要安装 Extension Pack 。
2. 在虚拟机设置中[开启 EFI](https://www.makeuseof.com/set-up-efi-linux-virtual-machine-virtualbox/) 并将 USB 控制器选择为 [USB 3.0](https://techsviewer.com/how-to-install-virtualbox-extension-pack-and-enable-usb-3-0/) 。

## 桌面安装与配置

安装轻量级 Xfce4 桌面: `sudo apt install xfce4` ，其中 Display Manager 选择 `lightdm` 。

电源: `xfce4-power-manager`
蓝牙: `bluetooth` 和 `blueman`
剪贴板: `xfce4-clipman`

在 Ubuntu 20.04 下不需要安装 `xfce4-screensaver`，有 `light-locker` 了。

配置锁屏: Settings -> KeyBoard -> Shortcuts，为 `xflock4` 命令添加快捷键 Super + L 。

## fcitx 输入法

```
sudo apt install fcitx fcitx-googlepinyin
```

在家目录创建 `.xprofile` ，内容写入：

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

（对于 Gnome 桌面的 Ubuntu）系统设置的语言支持中如默认没有中文支持需添加上，并设置默认输入法为 `fcitx` 。

系统托盘上找到小键盘，右键 Configure 添加中文 Google Pinyin 输入法。如果开机之后系统托盘没有出现 fcitx 小键盘，可在 Applications/Accessories/Fcitx 手动启动，或者用 Settings/Session and Startup 添加 fcitx 为开机启动项。（在 Gnome 桌面下是 Startup Applications ）

从安装输入法到能够正常使用，中途可能需要重启系统。

## 中文字体

安装中文字体

```
sudo apt install fonts-noto fonts-wqy-microhei fonts-wqy-zenhei
```

解决部分中文汉字显示为日文字形 (如 "门", "关", "复", "径", "将" 等):

修改 `/etc/fonts/conf.d/64-language-selector-prefer.conf`，将简体中文 (SC) 的优先级提高。

```
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
    <alias>
        <family>sans-serif</family>
        <prefer>
            <family>Noto Sans CJK SC</family>
            <family>Noto Sans CJK TC</family>
            <family>Noto Sans CJK HK</family>
            <family>Noto Sans CJK JP</family>
            <family>Noto Sans CJK KR</family>
            <family>Lohit Devanagari</family>
        </prefer>
    </alias>
    <alias>
        <family>serif</family>
        <prefer>
            <family>Noto Serif CJK SC</family>
            <family>Noto Serif CJK TC</family>
            <family>Noto Serif CJK JP</family>
            <family>Noto Serif CJK KR</family>
            <family>Lohit Devanagari</family>
        </prefer>
    </alias>
    <alias>
        <family>monospace</family>
        <prefer>
            <family>Noto Sans Mono CJK SC</family>
            <family>Noto Sans Mono CJK TC</family>
            <family>Noto Sans Mono CJK JP</family>
            <family>Noto Sans Mono CJK KR</family>
            <family>Noto Sans Mono CJK HK</family>
        </prefer>
    </alias>
</fontconfig>
```

## 网络管理

在 20.04 下 Xfce4 桌面自带网络管理器（在 18.04 需要手动安装 `wicd`）。

解决有线网卡 `device not managed` 的问题：

修改 `/usr/lib/NetworkManager/conf.d/10-globally-managed-devices.conf`

```
[keyfile]
unmanaged-devices=none
# 将 unmanaged-devices 设置为 none
```

然后用 `nmcli` 命令将网卡设置为 managed

```
sudo nmcli device set eth0 managed yes # eth0 换成实际的有线网卡名
```

完成后重启计算机或重启 `network-manager` 服务: `sudo systemctl restart network-manager.service`

## 挂载本地硬盘

Thunar 挂载本地硬盘时提示权限不足:

修改 `/usr/share/polkit-1/actions/org.freedesktop.UDisks2.policy` 文件，将 `org.freedesktop.udisks2.filesystem-mount-system` 的配置改为如下

```xml
<defaults>
  <allow_any>auth_admin</allow_any>
  <allow_inactive>auth_admin</allow_inactive>
  <allow_active>yes</allow_active>
</defaults>
```

(将 `allow_active` 原本的 `auth_admin_keep` 修改为 `yes`）

## root 授权

xfce4 桌面默认貌似不自带授权组件 (弹出对话框要求输入密码的功能)，如果 GUI 程序运行中需要 root 权限则可能报错，需要安装授权组件:

```
sudo apt install policykit-1 policykit-1-gnome
```


## 从旧的 Ubuntu 系统迁移数据

Ubuntu Server 18.04 和 20.04 的系统盘根目录采用 LVM 分区（例如 `/dev/sdc3` 的分区为 `ubuntu--vg ubuntu--lv`（前面是分组名，后面是分区名）

LVM 分区相关的命令: `vgscan`, `lvs`, `lvscan`, `lvmdiskscan`, `vgdisplay`。

1. `vgdisplay` 获取系统上已有 LVM 分区的 UUID。
2. `vgrename` 将指定 UUID 的分区重命名（当前 Ubuntu 和旧的 Ubuntu 系统盘如果分区结构相同，LVM 分组会重名）

        vgrename AAAAAA-AAAA-AAAA-AAAA-AAAA-AAAA-AAAAAA ubuntu-old
    
    示例中间的参数应替换成实际分区的 UUID。将旧系统盘的 `ubuntu--vg` 分区组重命名为 `ubuntu-old` 以便和当前系统做区分。
3. `vgchange -a y` 激活 LVM 分区

而后即可用 `mount` 命令挂载旧分区: `sudo mount /dev/ubuntu-old/ubuntu-lv /mnt/ubuntu-old/`

## 锁屏后输入密码界面卡死

由于系统内同时安装并激活了 `light-locker` 和 `xfce4-screensaver` 引起，卸载 `xfce4-screensaver` 或者在 Settings/Session and Starup 中禁用上述二者之一。

## Snap 相关

snap 没有国内源，需配置代理以加速软件包下载:

```
sudo snap set system proxy.http="http://127.0.0.1:20171"
sudo snap set system proxy.https="http://127.0.0.1:20171"
```

其中 `proxy.https` 的值内仍然使用 `http` ，端口号根据自己使用的代理软件的实际配置调整。

安装 `snap-store-proxy` 和 `snap-store-proxy-client` :

```
sudo snap install snap-store-proxy
sudo snap install snap-store-proxy-client
```

在 Ubuntu 22.04 中，浏览器 firefox 和 chromium 都只能通过 snap 方式安装，即使执行 apt 命令也是在安装过程中调用 snap 。如果直接用 Ctrl+C 方式暂停 apt 安装，可能导致 snap 进程残留(从而无法重新安装)。

清理方法:

```
snap changes            # 找到状态为 Doing 的任务 ID
sudo snap abort $id     # `$id` 替换成实际的任务 ID, 而后状态从 Doing 变为 Error 即可
```

## Shell 相关

Fish 可直接安装: `sudo apt install fish` ，无需任何配置，开箱即用。

Zsh 安装: `sudo apt install zsh` ，配置使用 [oh-my-zsh](https://ohmyz.sh/) :

1. 安装 oh-my-zsh

        git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
2. 复制 `.zshrc` 模板

        cp .oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
3. 下载高亮和自动补全插件
        
        cd ~/.oh-my-zsh/custom/plugins
        git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
        git clone https://github.com/zsh-users/zsh-autosuggestions.git
4. 修改 `.zshrc` ，配置主题与插件

        # 选择喜欢的主题
        ZSH_THEME="candy"
        # 插件激活列表
        plugins=(git zsh-autosuggestions zsh-syntax-highlighting)

## 其他软件

[VSCode](https://code.visualstudio.com/docs/setup/linux) : 按步骤添加 apt 源以及导入密钥。安装后如需使用 GitHub 登录同步设置需要安装 `gnome-keyring` 。

[Docker](https://docs.docker.com/engine/install/ubuntu/) : 按步骤添加 apt 源以及导入密钥。安装后将当前用户加入 `docker` 用户组从而无需用 root 用户: `sudo gpasswd -a $USER docker` ，执行后应重启系统，或执行 `newgrp docker` 临时刷新用户组配置。

Jetbrains 系列 IDE: 使用 snap 安装

```
sudo snap install intellij-idea-ultimate datagrip goland
```

AppImage 格式的软件 (以 Mark Text 为例): 

1. 先添加可执行权限 `chmod +x MarkText.AppImage` 。
2. 解压 AppImage: `./MarkText.AppImage --appimage-extract` ，得到名为 `squashfs-root` 的目录。
3. 将该目录中的快捷方式文件 (后缀名为 `.desktop`) 添加到系统应用菜单

        sudo cp squashfs-root/marktext.desktop /usr/share/applications
4. 将 AppImage 解压后的内容复制到 `/opt/marktext` 或系统中其他目录 (`/opt` 目录通常放置第三方软件)，并在 `/usr/bin` 中创建软链接。

        sudo mv squashfs-root marktext; sudo cp -r marktext /opt/
        sudo ln -s /opt/marktext/AppRun /usr/bin/marktext
5. 如系统应用菜单中快捷方式图标显示不正确，修改 desktop 文件中的 `Icon` 参数，指向图片文件:
    修改 `/usr/share/applications/marktext.desktop` :
    
        Icon=/opt/marktext/.DirIcon
