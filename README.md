# Hackintosh-Z490-ASRock-Steel-Legend-Intel-i7 10700

| 硬件配置 ||
|---|----------------------------------|
| 主板  | 华擎（ASRock）Z490 Steel Legend 钢铁传奇 |
|CPU|Intel i7-10700                      |
|内存|海盗船 DDR4 3200 16G X 2|
|显卡|蓝宝石（Sapphire）RX 5600XT|
|SSD|西数 SN750 500G X 2|


macOS Catalina 10.15.5

按照官方指引 Comet Lake 做的引导

OpenCore 0.5.9 

https://dortania.github.io/OpenCore-Desktop-Guide/

* 官方文档中在 “Creating the USB” --- "Windows install" 中创建的安装U盘是恢复镜像，安装过程中必须要有网络，我这里没有安装黑苹果免驱的无线网卡，板载2.5G网卡驱动需要手工配置才可使用，试过好象在安装界面也无法修改网络配置，所以这里必须用其它工具或者用 “macOS install” 的方法创建完整的安装镜像才可以正常安装

-------
2020.07.05

## 目前已知问题：
* Bios中开启USB键盘鼠标唤醒后，休眠可以用键鼠正常唤醒，但是关机会卡住无法关机

-------
## 相关配置说明：
ACPI
1. SSDT-AWAC.aml
2. SSDT-EC.aml
3. SSDT-PLUG.aml
4. SSDT-USBX.aml 
5. SSDT-PMCR.aml （修正节能第5项，如果不加默认是4项，非强迫症可忽略）

Kexts
1. AppleALC.kext
2. FakePCIID_Intel_HDMI_Audio.kext  板载ALC1200需要仿冒声卡才可以驱动
3. FakePCIID.kext
4. Lilu.kext
5. LucyRTL8125Ethernet.kext   板载2.5G网卡驱动（网卡设置“高级-硬件”中将速率改为1000baseT）
6. SMCProcessor.kext
7. SMCSuperIO.kext
8. USBPorts.kext
9. VirtualSMC.kext
10. WhateverGreen.kext

* 在使用USBInjectAll.kext的时候发现系统USB设备HS13被识别为LED Controller 应该和主板的LED光效有关，重启进入Bios后会造成Bios中的主板LED光效开关失效，只能重新定制USBPort.kext，将HS13设备排除
* 关于显卡优化，现在只找到针对5700XT的显卡的优化参数，5600XT据说也能用，我这里也没试，有兴趣的可以自己尝试，试过加载 RadeonBoost.kext发现并没有什么效果，原作者在最新版本也把5000系显卡的支持去掉了
https://www.insanelymac.com/forum/topic/343461-kext-tired-of-low-geekbench-scores-use-radeonboost/

-------
CPU，内存，显卡识别正常

![](ScreenShot/1.png)

CINEBENCH R20跑分，CPU变频正常

![](ScreenShot/2.png)

核显硬件加速正常

![](ScreenShot/3.png)

修眠正常

![](ScreenShot/4.png)


Geekbench CPU和显卡测试

![](ScreenShot/5.png)