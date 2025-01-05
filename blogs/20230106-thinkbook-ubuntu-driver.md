# ThinkBook 16+ Ubuntu 驱动问题

2023-01-06

在 ThinkBook 16+ 轻薄本上安装 Ubuntu 20.04 + Win11(出厂自带) 双系统，在 Linux 下无线网卡和蓝牙默认是缺失驱动的，需要手动安装，在此记录一下。

## Wi-Fi 驱动

```bash
git clone https://github.com/HRex39/rtl8852be.git
cd rtl8852be
make -j$(nproc)
sudo make install
# 执行 modprobe 命令后无需重启即可无线上网
sudo modprobe 8852be
```

## 蓝牙驱动

```bash
git clone https://github.com/HRex39/rtl8852be_bt.git
cd rtl8852be_bt
make -j$(nproc)
sudo make install
# 安装好蓝牙驱动后需要重启
sudo reboot
```

## 注意事项

如果 apt 更新了 Linux 内核，每次更新过内核后再重启都需要重新安装上述两个驱动（将驱动代码目录存放在本地，下次无需联网下载）。