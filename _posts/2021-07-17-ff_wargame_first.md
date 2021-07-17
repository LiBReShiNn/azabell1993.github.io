---
title:  "wargame play"
excerpt: "LINUX BASIC"
header:

categories:
  - C/C++
tags:
  - C/C++
---



해킹 입문 게임 플레이 웹 사이트 : https://overthewire.org/wargames/bandit/  

사실 해킹 입문이라기보다는 리눅스 입문 같아보이긴한다만... 보안쪽에서 국룰로 전통적인 입문 단계인 것 같아요.  
하드웨어 시스템 엔지니어가 목표이기 때문에 보안도 같이 공부합니다.  

(비밀번호를 획득하여서 정답을 적어놓지만 왠만하면 보지말고 직접 푸세요. 6단계까지는 너무 기본적이라서 풀이과정을 적지 않았지만 나머지는 모를수도 있기 때문에 풀이과정을 적었습니다. 파트를 두개로 나눠서 절반씩 올려놓겠습니다. :)  )   


홈페이지에 들어가면 보이는 왼쪽 메뉴에서 오른쪽 숫자를 기준으로 단계를 적었습니다.  
예)  Level0 -> Level1 이면  1단계 비밀번호 입니다.  

**0단계 비밀번호)** bandit0  
**1단계 비밀번호)** boJ9jbbUNNfktd78OOpsqOltutMc3MY1  
**2단계 비밀번호)** CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9  
**3단계 비밀번호)** UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK  
**4단계 비밀번호)** pIwrPrtPN36QITSp3EQaw936yaFoFgAB  
**5단계 비밀번호)** koReBOKuIDDepwhWk7jZC0RTdopnAYKh  
**6단계 비밀번호)** DXjZPULLxYr17uwoI01bNLQbtFemEgo7  
**7단계 비밀번호)** HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs 
```c
(find / -user bandit7 -group bandit6 -size 33c 2>/dev/null)  
```  
  
**8단계 비밀번호)** millionth / cvX2JJa4CFALtqS87jk27qwqGhBM9plV 
```c
(grep -r 'millionth' data.txt)  
```  
  
**9단계 비밀번호)** 1 UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR  
```c
(sort data.txt | uniq -u | sort -nrqie lines )  
```  
  
**10단계 비밀번호)** truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk   
```c
(strings data.txt | grep "===" |  sed s/==========//g | sed s/Z\)//g | tr -d '&')  
```  
  
**11단계 비밀번호)** The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR 
```c 
(base64 -d data.txt)  
```  
  
**12단계 비밀번호)** The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu 
```c
(cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m')  
```  
  
**13단계 비밀번호)** The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL   
  
**Level.13 step**  
  
```c
$ bandit12@bandit:~$ mkdir /tmp/exam
$ bandit12@bandit:~$ cd /tmp/exam
$ bandit12@bandit:/tmp/exam$ pwd
	/tmp/exam
$ bandit12@bandit:/tmp/exam$ cp /home/bandit12/data.txt /tmp/exam
$ bandit12@bandit:/tmp/exam$ ls
	data.txt
$ bandit12@bandit:/tmp/exam$ xxd -r data.txt > bandit
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data.txt
$ bandit12@bandit:/tmp/exam$ file bandit
	bandit: gzip compressed data, was "data2.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
$ bandit12@bandit:/tmp/exam$ cp bandit data2.bin.gz
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data2.bin.gz  data.txt
$ bandit12@bandit:/tmp/exam$ file data2.bin.gz
	data2.bin.gz: gzip compressed data, was "data2.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
$ bandit12@bandit:/tmp/exam$ file data2.bin
	data2.bin: cannot open `data2.bin' (No such file or directory)
$ bandit12@bandit:/tmp/exam$ gunzip data2.bin.gz
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data2.bin  data.txt
$ bandit12@bandit:/tmp/exam$ file data2.bin
	data2.bin: bzip2 compressed data, block size = 900k
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data2.bin  data.txt
$ bandit12@bandit:/tmp/exam$ mv data2.bin data3.bin.bz2
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data3.bin.bz2  data.txt
$ bandit12@bandit:/tmp/exam$ bunzip data3.bin.bz2
	-bash: bunzip: command not found
$ bandit12@bandit:/tmp/exam$ bunzip2 data3.bin.bz2
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data3.bin  data.txt
$ bandit12@bandit:/tmp/exam$ file data
	data3.bin  data.txt
$ bandit12@bandit:/tmp/exam$ file data
	data3.bin  data.txt
$ bandit12@bandit:/tmp/exam$ file data3.bin
	data3.bin: gzip compressed data, was "data4.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
$ bandit12@bandit:/tmp/exam$ mv data3.bin data4.bin.gz
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data4.bin.gz  data.txt
$ bandit12@bandit:/tmp/exam$ gunzip data4.bin.gz
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data4.bin  data.txt
$ bandit12@bandit:/tmp/exam$ file data4.bin
	data4.bin: POSIX tar archive (GNU)
$ bandit12@bandit:/tmp/exam$ mv data4.bin data5.bin.tar
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data.txt
$ bandit12@bandit:/tmp/exam$ tar -xvf data5.bin.tar
	data5.bin
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin  data5.bin.tar  data.txt
$ bandit12@bandit:/tmp/exam$ file data5.bin
	data5.bin: POSIX tar archive (GNU)
$ bandit12@bandit:/tmp/exam$ mv data5.bin data6.bin.tar
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin.tar  data.txt
$ bandit12@bandit:/tmp/exam$ tar -xvf data6.bin.tar
	data6.bin
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin  data6.bin.tar  data.txt
$ bandit12@bandit:/tmp/exam$ file data6.bin
	data6.bin: bzip2 compressed data, block size = 900k
$ bandit12@bandit:/tmp/exam$ mv data6.bin data7.bin.bz2
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin.tar  data7.bin.bz2  data.txt
$ bandit12@bandit:/tmp/exam$ file data7.bin.bz2
	data7.bin.bz2: bzip2 compressed data, block size = 900k
$ bandit12@bandit:/tmp/exam$ bunzip2 data7.bin.bz2
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin.tar  data7.bin  data.txt
$ bandit12@bandit:/tmp/exam$ file data7.bin
	data7.bin: POSIX tar archive (GNU)
$ bandit12@bandit:/tmp/exam$ mv data7.bin data8.bin.tar
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin.tar  data8.bin.tar  data.txt
$ bandit12@bandit:/tmp/exam$ tar -xvf data8.bin.tar
	data8.bin
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin.tar  data8.bin  data8.bin.tar  data.txt
$ bandit12@bandit:/tmp/exam$ file data8.bin
	data8.bin: gzip compressed data, was "data9.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
$ bandit12@bandit:/tmp/exam$ mv data8.bin data9.bin.gz
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin.tar  data8.bin.tar  data9.bin.gz  data.txt
$ bandit12@bandit:/tmp/exam$ gunzip data9.bin.gz
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin.tar  data8.bin.tar  data9.bin  data.txt
$ bandit12@bandit:/tmp/exam$ file data9.bin
	data9.bin: ASCII text
$ bandit12@bandit:/tmp/exam$ ls
	bandit  data5.bin.tar  data6.bin.tar  data8.bin.tar  data9.bin  data.txt
$ bandit12@bandit:/tmp/exam$ cat data9.bin
	The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```  

