---
title:  "아치리눅스에 윈도우10을 설치하고 USB를 연동시키는 환경 설정을 하자."
excerpt: "C programming makefile"
header:

categories:
  - C/C++
tags:
  - C/C++
---

국비학원이 아무래도 윈도우 기반이다 보니 가끔 윈도우 10을 사용할 일을 대비하여서 어쩔 수 없이 눈물을 머금고 윈도우 10을 가상머신으로 설치하였다.  
그런데 생각치도 못한 일이 일어났다.  
키보드, 마우스가 그대로 연동이 된다고 하여서 자료를 이동할 USB가 된다는 건 아니었다.  
  
그래서 나는 따로 환경설정을 구축하였고, 성공하였다.  
  
순서대로 진행을 하면 될 것 같다.  
  
```c
/etc/group
```  
  
```c
sudo usermod -a -G vboxusers "$USER"
```  
  
![Screenshot from 2021-06-12 22-39-40](https://user-images.githubusercontent.com/75885992/121780173-25a7e000-cbda-11eb-9a1a-dc05ac0d926a.png)  
  

위로 sudo vim이든 바로 넣든 둘 중 하나를 선택하여서 vboxusers:x:1000에 컴퓨터 이름을 삽입하면 된다.  
컴퓨터 이름이 azabell이므로 azabell이 삽입된 것을 확인 할 수 있다.  
  
![Screenshot from 2021-06-12 23-49-06](https://user-images.githubusercontent.com/75885992/121779943-15433580-cbd9-11eb-82b1-6b603530b595.png)  
  

```c
SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", MODE="0666", IMPORT{builtin}="usb_id", IMPORT{builtin}="hwdb --subsystem=usb"
ENV{MODALIAS}!="", IMPORT{builtin}="hwdb --subsystem=$env{SUBSYSTEM}"
```  
  

```c
$ /lib/udev/rules.d
```  
```c
$ sudo vim 50-udev-default.rules
```  
  
```c
 azabell@azabellLaptop  /lib/udev/rules.d  ls
01-md-raid-creating.rules   60-block.rules                    61-gdm.rules                           69-md-clustered-confirm-device.rules  80-drivers.rules                 95-cd-devices.rules
10-dm.rules                 60-cdrom_id.rules                 61-gnome-settings-daemon-rfkill.rules  70-infrared.rules                     80-libinput-device-groups.rules  95-dm-notify.rules
11-dm-lvm.rules             60-drm.rules                      61-mutter.rules                        70-joystick.rules                     80-net-setup-link.rules          95-upower-csr.rules
13-dm-disk.rules            60-evdev.rules                    63-md-raid-arrays.rules                70-memory.rules                       80-udisks2.rules                 95-upower-hidpp.rules
39-usbmuxd.rules            60-fido-id.rules                  64-btrfs-dm.rules                      70-mouse.rules                        84-nm-drivers.rules              95-upower-hid.rules
40-gphoto.rules             60-input-id.rules                 64-btrfs.rules                         70-power-switch.rules                 85-nm-unmanaged.rules            95-upower-wup.rules
40-monitor-hotplug.rules    60-persistent-alsa.rules          64-md-raid-assembly.rules              70-spice-webdavd.rules                90-bolt.rules                    96-e2scrub.rules
40-usb-media-players.rules  60-persistent-input.rules         65-kvm.rules                           70-touchpad.rules                     90-brltty-device.rules           99-fuse3.rules
49-stlinkv1.rules           60-persistent-storage.rules       65-libwacom.rules                      70-uaccess.rules                      90-brltty-uinput.rules           99-fuse.rules
49-stlinkv2-1.rules         60-persistent-storage-tape.rules  65-sane.rules                          71-ipp-usb.rules                      90-libinput-fuzz-override.rules  99-laptop-mode.rules
49-stlinkv2.rules           60-persistent-v4l.rules           66-saned.rules                         71-seat.rules                         90-nm-thunderbolt.rules          99-systemd.rules
49-stlinkv3.rules           60-rfkill.rules                   69-cd-sensors.rules                    73-seat-late.rules                    90-pipewire-alsa.rules           README
50-udev-default.rules       60-sensor.rules                   69-dm-lvm-metad.rules                  75-net-description.rules              90-pulseaudio.rules
51-android.rules            60-serial.rules                   69-libftdi.rules                       75-probe_mtd.rules                    90-udisks2-zram.rules
60-autosuspend.rules        60-vboxdrv.rules                  69-libmtp.rules                        78-sound-card.rules                   90-vconsole.rules
 azabell@azabellLaptop  /lib/udev/rules.d  sudo vim 50-udev-default.rules
```   
  
  
```c
SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", MODE="0666", IMPORT{builtin}="usb_id", IMPORT{builtin}="hwdb --subsystem=usb"
ENV{MODALIAS}!="", IMPORT{builtin}="hwdb --subsystem=$env{SUBSYSTEM}"
```  

위와 같이 MODE="0666"을 추가해준다.  
  
이해하기 쉽게 사진을 첨부해보겠다.  
  

![Screenshot from 2021-06-12 22-32-56](https://user-images.githubusercontent.com/75885992/121780049-81be3480-cbd9-11eb-9717-8bb58b330ec5.png)  
  
그렇게 되면 기본적인 셋팅은 전부 마친거다.  
마지막 남은 단계는 그냥 마우스 버튼 클릭으로 GUI환경에서 하면 되니까 누워서 떡먹기다.  
  
http://www.virtualbox.org/  
에 들어가서 자신의 버전에 맞는 확장팩을 필히 설치한다.  
환경설정의 usb탭에서 USB 3.0을 클릭해주면 된다.  


홈페이지를 들어가면  
  
**VirtualBox 6.1.22 Oracle VM VirtualBox Extension Pack**
```c
 All supported platforms
Support for USB 2.0 and USB 3.0 devices, VirtualBox RDP, disk encryption, NVMe and PXE boot for Intel cards. See this chapter from the User Manual for an introduction to this Extension Pack. The Extension Pack binaries are released under the VirtualBox Personal Use and Evaluation License (PUEL). Please install the same version extension pack as your installed version of VirtualBox.
```  
이 보이는데 여기서 All supported platforms를 클릭해준후 다운로드를 하여서 실행을하면 버추얼박스가 자동 업그레이드 된다.  
  
이 세가지 과정을 마치니 버추얼 머신에 성공적으로 USB인식이 되었다.  
   

