---

title:  "리눅스 환경 스크립트 파일 작성하기_기초_1(ip주소를 보여주는 스크립트 파일을  만들어보자)"
excerpt: "make *.sh file"
header:

categories:
  - C/C++
tags:
  - C/C++
---

*자신의 컴퓨터의 이름과 아이피를 보여주는 스크립트 파일을 만들어주는 실습을 해봅니다.*  

**전체 소스**  
```c
#!/bin/bash
#this *.sh file is get the computer ip address
#Licence : http://github.com/Azabell1993

trap 'printf "\n";stop' 2

echo "======================================="
echo "---------------------------------------"
echo "github : http://github.com/Azabell1993 "
echo "---------------------------------------"
echo "======================================="
echo "------######---####\-------------------"
echo "--------##-----#--#|-------------------"
echo "--------##-----#--#|-------------------"
echo "--------##-----####/-------------------"
echo "--------##-----#-----------------------"
echo "--------##-----#-----------------------"
echo "------######--###----------------------"
echo "======================================="
printf "\n"
echo "-----streaming..........---------------"
printf "\n"

catch_name() {
	echo -n "USER NAME : "
	whoami > whoami.txt
	cat whoami.txt
	cat whoami.txt >> whoami_saved.txt
	printf "\n"
}

catch_ip()
{
	echo -n "Your computer IP : "
	printf "\n"
	ifconfig | grep 'inet ' > ip.txt
	cat ip.txt
	cat ip.txt >> ip_saved.txt
}

catch_name
catch_ip
```

**실습 후 작동 모습**  
```c
 ✘ azabell@azabell  ~/Desktop/sh_pratice  ./read_on_ip.sh
=======================================
---------------------------------------
github : http://github.com/Azabell1993
---------------------------------------
=======================================
------######---####\-------------------
--------##-----#--#|-------------------
--------##-----#--#|-------------------
--------##-----####/-------------------
--------##-----#-----------------------
--------##-----#-----------------------
------######--###----------------------
=======================================

-----streaming..........---------------

USER NAME : azabell

Your computer IP :
        inet 211.217.79.76  netmask 255.255.255.0  broadcast 211.217.79.255
        inet 127.0.0.1  netmask 255.0.0.0
 azabell@azabell  ~/Desktop/sh_pratice  ls
ip_saved.txt  ip.txt  read_on_ip.sh  whoami_saved.txt  whoami.txt
 azabell@azabell  ~/Desktop/sh_pratice  cat ip_saved.txt
        inet 211.217.79.76  netmask 255.255.255.0  broadcast 211.217.79.255
        inet 127.0.0.1  netmask 255.0.0.0
 azabell@azabell  ~/Desktop/sh_pratice  cat ip.txt
        inet 211.217.79.76  netmask 255.255.255.0  broadcast 211.217.79.255
        inet 127.0.0.1  netmask 255.0.0.0
 azabell@azabell  ~/Desktop/sh_pratice  cat whoami_saved.txt
azabell
 azabell@azabell  ~/Desktop/sh_pratice  cat whoami.txt
azabell
```



  
실습을 할 자신이 원하는 폴더명을 만들어줍니다.  
```c
 azabell@azabell  ~/Desktop  mkdir sh_pratice
```  

리눅스 환경에서 자신의 컴퓨터 이름을 알아내는 명령어는 주로  
```c
$ whoami
```  
이고,  

아이피 주소를 알아내는 명령어는 주로  
```c
$ ifconfig
```
입니다.  

여기서 주의할 것은 아이피 주소를 ifconfig 명령어를 사용하여서 불러오고자 할 때 명령어가 먹히지 먹히지 않을 수가 있습니다.  

그럴때에는  
```
$ sudo apt-apt install net-tools
```
를 선행작업해줘야 합니다. 그러면 ifconfig를 이용하여서 해당 패키지를 운로드를 해주면 ifconfig 명령어를 사용할 수 가 있습니다.  

그럼 이제 스크립트 파일을 만들어줄 차례입니다.  
함수를 이용해서 작성을 해볼 것인데, 다른 언어 프로그램이랑 약간의 차이가 있습니다.  

함수명은 임의로 지정을 하여도 가능하지만, 함수명을 밑에다가 적어줘야 실행이 된다는 차이점이 있습니다.  
아래와 같이 작성을 하시면 됩니다.  

```c
function name() {
	sudo apt-get update
		..이외....
}

name
```  

이런 식으로 작성을 하시면 스크립트 파일 작성을 하시면

```c
$ chmod +x file.sh
```  
를 이용하여 쓰기/읽기 권한을 주고나서  

```c
$ ./file.sh
```  

로 함수안에 넣은 명령어가 작동이 가능하게 됩니다.  

이제 본격적으로 호스트 명과 아이피 주소를 불러오는 함수를 작성하여봅시다.  

```c
catch_name() {
	echo -n "USER NAME : "
	whoami > whoami.txt
	cat whoami.txt
	cat whoami.txt >> whoami_saved.txt
	printf "\n"
}
```  

하나씩 설명을 해주겠습니다.
echo 명령어는 쉘에다가 그냥  
```c
$ echo "hello world"
```
만 하여도 hello world가 불러와지는 것을 볼 수 있습니다.  
그럼 이 명령어를 이용하여서 텍스트 파일을 만드는 명령어는 > 와 >>을 이용하시면 됩니다.  

  '>'은 한 번만 쓰기이고, '>>'은 계속하여서 이어쓰기를 의미합니다.  

즉, 함수 안의 내용은 이렇게 풀이가 됩니다.  
```c
1. echo 명령어로 "USER NAME"를 출력.
2. whomi 명령어로 출력되는 것을 whoami.txt에 한 번만 쓴다.
3. cat 명령어로 넣은 텍스트 파일을 읽어준다.
4. whoami명령어로 불러와준 것을 계속하여서 별도의 텍스트 파일에 써준다.
5. 개행을 해준다.
```  

여기까지 이해를 하였다면 다음의 함수도 직관적으로 이해가 가능할 것 입니다.  

```c
catch_ip()
{
	echo -n "Your computer IP : "
	printf "\n"
	ifconfig | grep 'inet ' > ip.txt
	cat ip.txt
	cat ip.txt >> ip_saved.txt
}
```  

여기서 다른 점이라면 grep이라는 명령어인데, 이는 불러올 명령어에서 특정 단어를 검색하여서  보여주도록 도와주는 기능입니다.  
아래에서 확실한 차이점을 확인할 수 있습니다.  

```c
 azabell@azabell  ~/Desktop/sh_pratice  ifconfig
enp6s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 211.217.79.76  netmask 255.255.255.0  broadcast 211.217.79.255
        inet6 fe80::9a4f:67cb:6eca:9ef5  prefixlen 64  scopeid 0x20<link>
        ether a8:5e:45:a3:03:b2  txqueuelen 1000  (Ethernet)
        RX packets 8589248  bytes 1960671480 (1.9 GB)
        RX errors 0  dropped 39137  overruns 0  frame 0
        TX packets 461693  bytes 55982730 (55.9 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 7876  bytes 818188 (818.1 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 7876  bytes 818188 (818.1 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

 azabell@azabell  ~/Desktop/sh_pratice  ifconfig | grep 'inet '
        inet 211.217.79.76  netmask 255.255.255.0  broadcast 211.217.79.255
        inet 127.0.0.1  netmask 255.0.0.0
```  

차이점이 보이시나요?  
자신의 리눅스로 직접 실습을 해보시면 더욱 이해가 쉽습니다.  

이외에도 많은 기능이 있으나, 다음 포스팅에서 설명을 해주겠습니다.  

감사합니다.  
