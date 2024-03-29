---
title:  "아치 리눅스 설치 (기본 셋팅 편)"
excerpt: "install_linux_1"
header:

categories:
  - C/C++
tags:
  - C/C++
---

![20210505_223936.jpg](https://user-images.githubusercontent.com/75885992/117601883-4f5c8a00-b18a-11eb-9cb0-669b8bceeebc.jpg)  

 우분투를 쓰고 있었지만 뭔가 허전함이 느껴졌고 필자는 아치리눅스를 쓰기로 하였다.  
 젠투도 있었지만 아치로도 충분할 것 같았다.  
 아치리눅스는 오로지 CLI를 통하여 설치를 하는 과정을 거쳐야한다.  
 결과적으로 설치가 잘됬고 필자는 우분투에서 아치로 완전히 넘어왔다.      
   
 중간에 꼬이지않아야 설치 진행에 있어서 에러를 먹지 않는다.  

 필자가 겪은 시행착오를 포함하여서 설치과정을 온전히 적어두고자 하여서 포스팅을 하였다.  

 **설치 usb 포맷시 반드시 파티션 방식을 GPT방식으로 해야하고 포맷 옵션의 파일시스템은 FAT32로 진행한다.**
 **그리고 ISO형식으로 하는게 아니라 DD형식으로 마무리를 해준다.**  

 필자의 경우에는 'Rufus'프로그램을 사용하여서 포맷용 usb를 만들었고, 다른 많은 사람들도 이 기능을 아용하여서 하는 것 같아보인다.  
 
 그렇지 않으면 설치 과정이 제대로 되지 않는다.  
 준비물 usb를 꽂고 부팅한다면 강아지인지 송아지인지 구별이 안가는 동물이 반겨준다.  
   
 그때 확인해야할 명령어는  

 ```c
 $ ls /sys/firmware/efi/efivars
 ```  
 를 입력하여준다.

 그럼 여러가지 내용이 나온다면 정상적으로 설치할 준비가 된 것이라고 볼 수 있다.  
 여기서 막힌다면 다시 설치 준비를 해와야할 것이다.  
 절대 그냥 넘어가면 안된다.  
 아직 CLI 환경이 어렵다면 GUI를 사용하는 만자로로 사용하면 될 것 같다.  
 우분투처럼 포맷을 대충했다가 다시했다.  
   
 와이파이를 잡아주는데 와이파이로 잡아도좋고 유선 렌선으로 잡아줘도 좋다.  
 필자 같은 경우에는 와이파이를 명령어로 잡아준 다음에 재부팅한 후에는 데스크탑 환경에서 와이파이를 다시 잡아줬다.  

 와이파이 잡는 과정은 하기에 적혀있는 것을 따라하면 되는데 순서대로 진행하면 될 것 같다.  

 ```c
 $ iwctl
 [iwd]$ device list
 [iwd]$ station wlan0 scan
 [iwd]$ station wlan0 get-networks

 [iwd]$ station wlan0 connect SSID  
 (input the passward)
 [iwd]$ exit

 $ ping -c 3 www.google.com
 ```  
 반드시 마지막에 ping으로 인터넷 연결을 확인해봐야한다.  
 윈도우에서도 cmd로 확인할 수 있는 방법인데, 먼저 cmd창을 열어준 후에 ipconfig로 IPv4에 나와있는 아이피를 확인해준다.  
 그리고 ping IPv4를 입력하여주면  

 Pinging 211.000.00.00 with 32 byte of data:  
 Reply from 211.000.00.00: bytes=32 time<1ms TTL=128  
 Reply from 211.000.00.00: bytes=32 time<1ms TTL=128  
 Reply from 211.000.00.00: bytes=32 time<1ms TTL=128  
 Reply from 211.000.00.00: bytes=32 time<1ms TTL=128  

 이렇게 나오는 것을 볼 수 있다.  
 여기서 bytes가 일정하게 나와야 인터넷 연결이 순조롭다고 볼 수 있다.  
 윈도우에서의 ping 명령어는 여기까지 알아보기로 하고, 다시 리눅스로 돌아오자.  

 만약 와이파이가 잘 잡히지 않는다면 다시 설치를 하거나, 그냥 유선으로 연결하면된다.  

 파티션 분할을 하는 과정이다.  
 윈도우에서 포맷을 할때는 cmd창을 띄워서 diskpart를 쳐서 list disk를 입력하여서 설치해줄 파티션 설치하여 포맷을 하면 됬지만 이건 리눅스다.  
 방식이 약간 다르다.  

 아래의 글에서 (엔터)라고 나와있는 부분은 어떤 문장이 뜨면 무시하고 그냥 엔터를 누르면 된다.  
 이 글은 리눅스에 어느 정도 익숙한 사람을 대상으로 쓴 포스팅이기 때문에 아예 포맷부터 시작을 하는 gdisk 명령어로 진행을 하기로 한다.
 
 ```c
 $ fdisk -l
 $ gdisk /dev/sda1
 Command (? for help) : o
 y
 ```  

 여기서부터 본격적으로 파티션을 나누는 작업을 진행하면 된다.  

 ```c
 n
 (enter)
 (enter)
 +512M (512MB 할당)
 ef00 (EFI system partition 할당)

 n
 (enter)
 (enter)
 +8G
 8200 (linux swap)

 n
 (enter)
 (enter)
 +85G
 (enter)

 n
 (enter)
 (enter)
 (enter)
 (enter)

 p (파티션 구성 확인)
 w (디스크에 작성)
 y
 ```  

 (설치가 진행되는 모습)  
 ![20210505_230809](https://user-images.githubusercontent.com/75885992/117610638-0b26b500-b19d-11eb-9048-6d0a50138352.jpg)  


 각각의 파티션을 포맷하고, swap파티션을 사용한다.  
 ```c
 $ mkfs.vfat -F32 /dev/sda1
 $ mkfs.ext4 /dev/sda3
 $ mkfs.ext4 /dev/sda4
 $ mkswap /dev/sda2
 $ swapon !$
 ```  

 파티션 마운트 (여기서는 리눅스를 설치할 디스크를 마운트하는 것이므로 외장하드나 다른 하드는 설치완료후 따로 mount를 해줘야 했다.)  
 ```c
 $ mount /dev/sda3 /mnt
 $ mkdir /mnt/boot
 $ mkdir /mnt/home
 $ mount /dev/sda1 /mnt/boot
 $ mount /dev/sda4 /mnt/home
 ```  

 필자는 리눅스에서 포맷을 하여서 아치리눅스를 설치하였기 때문에 sda1~4로 되었으나, 윈도우에서 진행을 한다면 nvme0n1p1로 표시가 될 수 있다.  
 각자 상황에 맞게 번호만 수정을 해주면 무리없이 진행이 될 수 있다.  

 **맨 마지막의 데스크탑 환경을 조성해준 후, 다시 여기로 돌아와서 외장하드나 연결한 다른 하드디스크를 열결하는 방법을 보시면 됩니다.**  
 ![Screenshot from 2021-05-10 12-54-07](https://user-images.githubusercontent.com/75885992/117603822-f17e7100-b18e-11eb-8f45-1662d9564d85.png)  
 먼저 gnome의 현경이 조성된다면 Untilities에 들어가준다.  
 그리고 위의 보여지는 이미지의 Disk Usage Analzer을 눌러주고 해당 디스크를 눌러주면 경로가 나오면서 mount를 해줘야한다고 뜬다.  
 위의 과정에서 해준 것처럼 똑같이 mount를 해주면 사용이 가능하게 된다.  

 **base package install**  
```c
$ pacstrap /mnt base linux linux-firmware nano networkmanager base-devel man-db man-pages texinfo
$ genfstab -U /mnt >> /mnt/etc/fstab
# arch-chroot /mnt
```  

**여기서는 bash쉘 공간입니다.**  
```c
$ ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
$ hwclock --systohc
$ passwd
$ nano /etc/locale.gen 
(en_US.UTF-8 UTF-8 앞의 주석을 지워주면 된다. 한글을 하면 꼬일까봐 영어로 깔았다.)  

$ locale-gen
$ echo LANG=en_US.UTF-8 > /etc/locale.conf

$ echo HOSTNAME(쓰고 싶은 컴퓨터 이름 작성) > /etc/hostname 
$ useradd -m -g users -G wheel -s /bin/bash HOSTNAME
$ passwd HOSTNAME
```  

**bootctl install**  
```c
$ bootctl install
$ nano /boot/loader/loader.conf
```  

**loader.conf file write**  
```c
default arch.conf
timeout 0
console-mode max
editor no
```  

**bootloader setting**  
```c
$ pacman -Syu intel-ucode (intel user)
$ pacman -Syu amd-ucode (amd user)
```  

**/boot/loader/entries/arch.conf** 내용 작성하기  
```c
title Arch Linux
linux /vmlinuz-linux (vmlinux 아닙니다.)
initrd /intel-ucode.img (intel user)
initrd /amd-ucode.img (amd user)
initrd /initramfs-linux.img
options root=/dev/sda3 rw
```  

```c
$ systemctl enable NetworkManager.service
$ exit

$ umount -R /mnt
```  

이제 리부트를 해주면된다.  

그런데 그냥 리부트를 해버리면 사용자 설정에 유저 등록이 안되어있기 때문에 root계정으로 들어가야 데스크탑 환경이 설치가 된다.  
우분투에서는 root로 들어가도 유저계정과 동일하게 파일이 있었으나 아치리눅스에서는 완전히 다른 개념이다.  

그렇기 때문에 **반드시** 이 과정을 거쳐야한다.  
필자가 이 과정을 안겪었기 때문에 관리자로 들어가서 진행을 하려고 했다가 방법을 몰라서 다시 포맷한 이후에 유저등록 방법을 알게 되고 설치를 진행하였었다.  

우선 *nano /etc/sudoers*로 들어가준다.  

root는 이미 되어있을 것이다.  

그렇다면 이제 사용자 본인이 설정하여준 이름을 그대로 갖다써주면 그만이다.  

```c
## User privilege specification
##
root ALL=(ALL) ALL
(USER NAME) ALL=(ALL) ALL
```  

그리고 진짜로 리부트를 한 번 해준다.  

절대 위의 bash과정에서 잘못 건들면 안된다.  
망가질 수 있다.  

그래서 아치리눅스는 우분투의 쉬운 리눅스부터 어느 정도 리눅스를 만져본 사람에게 권장을 해주고 싶다.  
아치와 젠투를 알 정도면 이미 리눅스에 익숙해졌다는 반증이기 때문에 천천히 따라하면 될 것 같다.    

```
$ reboot
```   

 
이제 기본적인 셋팅은 모두 끝난 것이고 데스크탑 환경을 설치할 차례이다.   


```c
$ sudo pacman -Syu xorg-server gnome
$ sudo systemctl enable gdm
$ reboot
```  

여러가지 테마가 있지만 개인적으로 gnome이 가장 깔끔해 보였다.  

이제 드라이버 셋팅을 시작해보겠다.  


**업데이트**    

```c
$ sudo pacman -Syu
```


우선 모든 소프트웨어를 설치하기전에 다음과 같은 패키지의 도움이 필요하므로 다운로드를 해준다.  
```c
$ sudo pacman -S bash-completion
$ sudo pacman -S xorg-server xorg-xinit xorg-utils xorg-server-utils mesa
$ sudo pacman -S xorg-twm xterm xorg-xclock
$ sudo pacman -S xf86-input-synaptics (laptop사용자의 경우 터치패드 드라이버)
```  

이제 비디오 카드 드라이버를 식별해야 한다.  
아래와 같은 명령어를 입력하여주자.  

```c
$  lspci | grep VGA
```  

그렇다면 이렇게 나올 것이다. (필자 노트북 예시)  


```c
azabell@azabellLaptop  ~  lspci | grep VGA
00:02.0 VGA compatible controller: Intel Corporation HD Graphics 530 (rev 06)
01:00.0 VGA compatible controller: NVIDIA Corporation GP106M [GeForce GTX 1060 Mobile] (rev a1)
```  

그래픽 카드 감지가 끝났다면...이제 위의 그래픽 카드를 찾아서 그래픽 드라이버를 설치하여주면 된다.  

우선 그래픽 드라이버가 어떤 것들이 있는지 알기 위해서는 검색을 해줘야한다.  

**nvidia user**  

```c
 azabell@azabellLaptop  ~  sudo pacman -Ss | grep nvidia
[sudo] password for azabell:
extra/nvidia 465.27-4 [installed]
extra/nvidia-dkms 465.27-1 [installed]
extra/nvidia-lts 1:465.27-3 [installed]
extra/nvidia-prime 1.0-4 [installed]
extra/nvidia-settings 465.27-1 [installed]
extra/nvidia-utils 465.27-1 [installed]
extra/opencl-nvidia 465.27-1 [installed]
community/nvidia-cg-toolkit 3.1-6 [installed]

```  

**intel user**  

```c
 azabell@azabellLaptop  ~  sudo pacman -Ss | grep intel
extra/intel-ucode 20210216-1 [installed]
extra/libva-intel-driver 2.4.1-1
extra/vulkan-intel 21.0.3-3
extra/xf86-video-intel 1:2.99.917+916+g31486f40-1 (xorg-drivers) [installed]
community/intel-compute-runtime 20.48.18558-1
community/intel-gmmlib 21.1.1-1
community/intel-gpu-tools 1.26-1
community/intel-graphics-compiler 1:1.0.5435-1
community/intel-media-driver 21.1.3-1
community/intel-media-sdk 20.5.1-1
community/intel-mkl 2020.4.304-1
community/intel-mkl-static 2020.4.304-1
community/intel-opencl-clang 11.0.0-2
community/intel-undervolt 1.7-2
community/intellij-idea-community-edition 4:2021.1.1-1
    An intelligent predictive text entry system
community/python-intelhex 2.3.0-2
    An intelligent Chinese Encoding converter
```  

**AMD/ATI user**  
```c
$ sudo pacman -Ss | grep ATI $ sudo pacman -Ss | grep AMD
```  

그리고 해당하는 그래픽 드라이버를 다운로드 해준다.  
```c
$ sudo pacman -S (그래픽 드라이버_예: nvida-dkms , nvidia-lts, dnvidia-prime) 
```  

그리고 성공적인 셋팅을 위해서 아래의 명령어를 입력해준다.  

```c
$ xrandr --auto
```  

연결이 잘되었는지 확인을 한 번 해준다.  

```c
$ xrandr
```  

여기까지 진행되었다면 기본적인 디스플레이와 데스크톱 환경이 셋팅된 것이다.  

아참, 그리고 yay라는 명령어가 있는데 aur 래퍼로써 이 명령어는 자주 쓰인다.  
빈 창에 yay를 입력해보고 설치가 안되어있다면 아래를 진행하여주면 된다.  

```c
$ sudo pacman -Syu
$ sudo pacman -S git
$ git clone https://aur.archlinux.org/yay.git
$ cd yay
$ makepkg -si
$ yay -S gparted
```  

```c
$ yay
```  

그놈 쉘 확장 명령어  
```c
$ sudo pacman -S gnome=shell-extensions gnoe-tweaks
```  

한글 입력을 도와주는 과정이다.  

```c
$ yay -S uim
```  

그리고 .xinitrc 경로에 들어가서 내용을 추가하여준다.  

```c
export GTK_IM_MODULE=uim
export QT_IM_MODULE=uim
uim-xim & export XMODIFIERS=@im=uim
```  

아래는 필자의 환경이다. 
```c
 azabell@azabellLaptop  ~  cat ~/.xinitrc
export GTK_IM_MODULE=uim
export QT_IM_MODULE=uim
uim-xim &
export XMODIFIERS=@im=uim

xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```  


이제 아래의 명령어를 수행한다.  
```c
uim-pref-gtk3
```  

아래의 이미지처럼 기본 uim을 byeru로 바꿔주고, 그놈 셋팅에서 키보드를 누른 후 한국어를 추가해준다.  
![Screenshot from 2021-05-10 14-02-20](https://user-images.githubusercontent.com/75885992/117608201-8e91d780-b198-11eb-8053-61e63bb61849.png)  
![Screenshot from 2021-05-10 14-02-36](https://user-images.githubusercontent.com/75885992/117608221-9baec680-b198-11eb-9f65-d7ad68f2e318.png)  

![Screenshot from 2021-05-10 14-01-36](https://user-images.githubusercontent.com/75885992/117608268-b4b77780-b198-11eb-8e34-b1301ed6ba73.png)  


한국어 설정이 완료되었다.  

NTFS 외장하드 마운트 방법  
```c
$ sudo pacman -S ntfs-3g
```  

블루투스 설치  
```c
$ sudo pacman -S bluez{,-utils}
$ sudo pacman enable bluetooth
$ sudo systemctl start !$
```  

로그인 화면 설정  
필자의 경우에는 lightdm으로 이쁘게 해주기 위해서 아래의 명령어를 실행하였다.  
오타나면 셋팅화면이 제대로 뜨지 않으니 Ctrl + Alt + f2 키를 눌러서 오탈자를 수정하고 reboot를 해주면 된다.

```c
$ sudo systemctl disable gdm
$ sudo pacman -S lightdm lightdm-webkit2-greeter
```  

**Greeter 사용 설정**
/etc/lightdm/lightdm.confg 경로로 들어가서 수정  

```c
[Seat:*]
...
greeter-session=lightdm-webkit2-greeter
...
```  

**lightdm 활성화시키기**  
```c
$ sudo systemctl enable lightdm
```  

**ip 주소 확인**  
```c
$ sudo pacman -S net-tools
$ sudo systemctl start NetworkManager
$ ifconfig
```  

이제 소프트웨어들만 깔면된다.  
다음 포스팅에서 소프트웨어를 설치하는 것을 참조하면 될 것 같다.  


