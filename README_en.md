# Hackintosh-Z490-ASRock-Steel-Legend-Intel-i7 10700

| Hardware ||
|---|----------------------------------|
| Motherboard  | ASRockZ490 Steel Legend |
|CPU|Intel i7-10700                      |
|Memory|USCORSAIR DDR4 3200 16G X 2|
|Graphics Card|SapphireRX 5600XT|
|SSD|WD SN750 500G X 2|
|Wifi/Bluetooth|Broadcom BCM94360CD|


macOS Catalina 10.15.7

The boot files are configured based on the official instructions for Comet Lake platform.

OpenCore 0.6.2 

https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html

* The image created from the instructions in official manual: “Creating the USB” --- "Windows install" is a recovery image only, you need Internet connection during installtion. I don't have a driver-free wireless card, and the onboard LAN is 2.5G, which must be configured to 1G manually during installation. To do it, open the terminal during installation and execute the following commands:
    `ifconfig en0 media 1000baseT ` （Thanks to[id86021](https://github.com/xiaoyaowx/Hackintosh-Z490-ASRock-Steel-Legend-Intel-10700/issues/2#issue-651397552)）

* You need to set your BIOS correctly as stated in the official instructions, otherwise, you might stuck at the installation phase. https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#intel-bios-settings

-------
2020.10.10
* Updated to OpenCore 0.6.2 and Kexts drivers
* The new AppleALC now supports the onboard audio, removed the fake driver, added boot parameter alcid=49
（iStat Menus does not show CPU temp correctly, but works with other software）

-------
2020.08.25
* Updated to OpenCore 0.6.0  and Kexts drivers


-------
2020.07.20
* Installed a new Broadcom BCM94360CD wireless card（4 antennas）, didn't to any change to EFI, both wifi and bluetooth are working.
* To fix wifi getting slow after wake up, you can disable "Wake for network access" option in "Engergy Saver" control pannel.
* Need to connect onboard 9-pin USB connectors to enable bluetooth function, need to customize the USB port.

-------
2020.07.07

* Injected graphics optimization parameters, applicable for both 5600/5700xt，injected ROM number and VBIOS version number.The previous quick way to load RadeonBoost.kext is now unusable for 5000 series graphics card.（https://www.insanelymac.com/forum/topic/343461-kext-tired-of-low-geekbench-scores-use-radeonboost/）

![](ScreenShot/7.png)

Geekbench score improves after this optimization, Final Cut Pro video export performance has a significant improvement (from 15s to 9s)

![](ScreenShot/6.png)

References:

https://www.tonymacx86.com/threads/amd-radeon-performance-enhanced-ssdt.296555/

http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1839725&highlight=5700xt

-------
2020.07.05

## Known issues:
* If you enable "Wakekup by USB Mouse, Usb Keyboard/Wakeup remotely, wakeup by PCIE device", trying to shut down the computer will hang.

-------
## Configurations explainations:
ACPI
1. SSDT-AWAC.aml
2. SSDT-EC.aml
3. SSDT-PLUG.aml
4. SSDT-USBX.aml 
5. SSDT-PMCR.aml （Fixed enger savin goption 5, without this will be option 4, can ignore if you are OK with it.)
6. SSDT-SBUS-MCHC.aml (Fixed SMBus support) 

Kexts
1. AppleALC.kext
2. FakePCIID_Intel_HDMI_Audio.kext  Need to fake audio device to enable onboard ALC1200板载ALC1200
3. FakePCIID.kext
4. Lilu.kext
5. LucyRTL8125Ethernet.kext  Onboard 2.5G ethernet card driver（You need to change link speed in "Advanced-Hardware" in network ethernet configuration to 1000baseT manually）
6. SMCProcessor.kext
7. SMCSuperIO.kext
8. USBPorts.kext
9. VirtualSMC.kext
10. WhateverGreen.kext

* When enabled USBInjectAll.kext, the system USB device HS13 is recognized as LED controller, which may be related to the onboard LED controller. Rebooting to BIOS will result in malfunction of the onboard LED switch, have to customize USBPort.kext to ignore HS13.


-------
CPU, Memory and Graphics card are all detected correctly.

![](ScreenShot/1.png)

CINEBENCH R20 score, CPU speedstep works normally.

![](ScreenShot/2.png)

Integrated graphics card acceletation works normally.

![](ScreenShot/3.png)

Hibernation works normally.

![](ScreenShot/4.png)


Geekbench CPU and graphic test result

![](ScreenShot/5.png)
