---

title:  "서울 기술 교육센터 C언어 클래스 시간 정리"
excerpt: "LINUX Programming"
header:

categories:
  - C/C++
tags:
  - C/C++
---

 저는 5월 28일에 서울 기술교육센터 임베디드반에 입학을 하여서 약 한 달 반의 교육을 받은 이력이 있는데  
학원의 개발 과제가 쉬워서 개인적으로 진행한 코드입니다.  
  
 

 Git Hub Address  
 ```c
 https://github.com/Azabell1993/KoreaChamberofCommerceAndIndustry
 ```  



 ```c
 대한상공회의소
(2021년 5월 28일 입학)

HostnameIpSh : Shell Script 기초 구현 연습 s
InstallArduinoUnoScriptFile_Linux : Linux에서의 프로그래밍을 위한 설치, 디렉토리 자동화 구현 자료
MathCaulatorProgram : 기탄수학 프로그램 버전 1은 main.c와 program.h 두 개만 있고 버전 2는 직접 타이핑해서 만든 linux API가 내장함수로 수록
PracticalExam : 실기시험 본 것들.
PraticeLib : 처음으로 라이브러리를 구현 해보고 만들어 본 것
RcCar : 입학시험으로 본 것. (우분투 리눅스에서 구현)
SwitchProgram : switch 문법 수업때 제출한 코드
Tetris : 테트리스
```  

저 중에서 그나마 가장 많은 공을 들인 코드는 테트리스 게임입니다.  
  
# **Tetris Game Program**  

***File Tree***
===============

```c
.
├── game_install.sh
├── include
│   ├── ename.c.inc
│   ├── error_functions.c
│   ├── error_functions.h
│   ├── get_num.c
│   ├── get_num.h
│   └── tlpi_hdr.h
├── logo.h
├── main.c
├── Makefile
├── move_down.h
├── move_left.h
├── move_right.h
├── move_rotate.h
├── Print_screen.h
├── program.h
├── README.md
├── settings.h
├── toplist_saevd.text
├── whoami_saved.text
└── whoami.text
```  

**Where is the use at 'include' folder**

```c
  if(!(f=fopen("toplist", "r")) == '\0')
  {
	  errExit(" File open Error \n");
  }
```

***Makefile***

```c
#
CC=gcc
CFLAGS= -Wformat -W -Wextra -Werror -Wall -I ./include/
COMPILE = $(CC) $(CFLAGS)
PATH_LIB= ./include/
SRCS = main.c
LIBS=-lncurses
MOBJ= main.o
OBJ = $(PATH_LIB)error_functions.o
ER= $(PATH_LIB)error_functions.c
NAME= main

.PHONY: depend clean

all: $(NAME)
	@echo  '$(NAME)' has been compiled. You can play the game through '$(NAME)'.

first:
	$(CC) -c -o $(OBJ) $(ER)

$(NAME): $(MOBJ)
	ar rc $(NAME)
	$(CC) $(CFLAGS) -o $(NAME) $(MOBJ) $(LIBS) $(OBJ)

clean:
	$(RM) *.o *~ $(NAME) $(PATH_LIB)*.o
	rm -rf toplist
#
```

***HOW TO PLAY THE GAME***
===============

**First Step**


```c
$ make fist
```

```c
gcc -c -o ./include/error_functions.o  ./include/error_functions.c
```


**Second Step**

```c
$ make
```

```c
gcc -Wformat -W -Wextra -Werror -Wall -I ./include/    -c -o main.o main.c
ar rc main
gcc -Wformat -W -Wextra -Werror -Wall -I ./include/  -o main main.o -lncurses ./include/error_functions.o
main has been compiled. You can play the game through main.
```

**Or play the game with an automated system**

```c
$ ./game_install.sh
```

```c
Mr/Ms. : (USER NAME)
Hello! Nice to meet you!

Running the Makefile
gcc -c -o ./include/error_functions.o  ./include/error_functions.c
gcc -Wformat -W -Wextra -Werror -Wall -I ./include/    -c -o main.o main.c
ar rc main
gcc -Wformat -W -Wextra -Werror -Wall -I ./include/  -o main main.o -lncurses ./include/error_functions.o
main has been compiled. You can play the game through main.
saved toplist

latest estimate User : (USER NAME)
(SHOW THE FILE)

Open the toplist log
(SHOW THE FILE)
```


**Result**
(make first, make stap)
```c
$ ls
game_install.sh  logo.h  main.c  Makefile     move_left.h   move_rotate.h   program.h  settings.h
include          main    main.o  move_down.h  move_right.h  Print_screen.h  README.md  toplist
```
(.sh step)
```c
game_install.sh  main    Makefile     move_right.h    program.h   toplist_saevd.text
include          main.c  move_down.h  move_rotate.h   README.md   whoami_saved.text
logo.h           main.o  move_left.h  Print_screen.h  settings.h  whoami.text
```

**play the main**

```c
$ ./main
```

(Program.h)
=========================================
![program](https://user-images.githubusercontent.com/75885992/125165420-53a32480-e186-11eb-8c6f-593e9b64cd1f.png)

(Before putting "$(OBJ)" in "Makefile")
=========================================

![NULL](https://user-images.githubusercontent.com/75885992/125165668-7550db80-e187-11eb-892e-b155e79d3b17.png)

(ETC / screencapture)
=========================================


![linux_console](https://user-images.githubusercontent.com/75885992/125164581-8f3bef80-e182-11eb-8fb4-4e58d1fead8c.png)

![Tetris_Makefile](https://user-images.githubusercontent.com/75885992/125164061-ceb50c80-e17f-11eb-92d3-c21a53c136a7.png)

![make_run](https://user-images.githubusercontent.com/75885992/125164127-323f3a00-e180-11eb-8028-6f603e2ef575.png)

![gameins](https://user-images.githubusercontent.com/75885992/125180315-18394200-e1e8-11eb-9284-28a172f08262.png)

![run_sh](https://user-images.githubusercontent.com/75885992/125164438-bcd46900-e181-11eb-9fb9-a9691929e282.png)

![ls](https://user-images.githubusercontent.com/75885992/125164168-6a467d00-e180-11eb-8f20-97a922662c0d.png)

![te1](https://user-images.githubusercontent.com/75885992/125160450-70caf980-e16c-11eb-87ce-5a5246e0e67e.png)



테트리스 게임을 만들 때 우선 간단하게 아래의 헤더파일처럼 작성을 해주었습니다.  

이 외에도 직접 타이핑하여 작성한 리눅스용 API도 파일 입/출력에서 활용을 할 수 있도록 간단한게 함수를 대입하였습니다.  
추후에 업데이트와 수정이 될 수 있습니다.  
아직 완성본이 아니라서요.  

  

```c
#ifndef PROGRAM_H_
#define PROGRAM_H_

#define _CRT_SECURE_NO_WARININGS

#define C_NRML "\033[0m"
#define C_BLACK "\033[30m"
#define C_RED	"\033[31m"
#define C_GREN 	"\033[32m"
#define C_YLLWW "\033[33m"
#define C_BLUE  "\033[34m"
#define C_PRPL  "\033[35m"
#define C_AQUA  "\033[36m"

/* include header */
#include <ctype.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/time.h>
#include <time.h>
#include <unistd.h>
#include <ncurses.h>
#include <fcntl.h>
#include <malloc.h>
#include <stddef.h>
#include <termios.h>
#include <stdbool.h>

#include "include/error_functions.h"
#include "include/get_num.h"
#include "include/tlpi_hdr.h"


#define WIDTH 	25
#define HEIGHT 	24
#define TOPLSITMAXLINELENGTH 34

char piece;
char *name;

int level;
int score;
int showtext=1;
int next;
int shownext = 1;
int end;
int clrlines = 0;
int startlevel;
int dropped = 0;

char key;

char left[HEIGHT][WIDTH] = {0};
char center[HEIGHT][WIDTH] = {0};
char right[HEIGHT][WIDTH] = {0};

int fixedpoint[2] = {0};

#define MOVL 'a'
#define MOVR 'd'
#define ROTA 'w'
#define DROP 's'
#define RSET 'l'
#define SNXT 'z'
#define STXT 'c'
#define EXT  'e'
#define TPLS 't'

#define NEMO 'n'
#define ONE 'o'

#define SCORE (100*cleard+75*(cleard/2)+125*(cleard/3)+150*(cleard/4))* level
#define DROPINTERVAL (DELAY/1000) * ((300 - level*13)/5)
#define DELAY 7777
#define MAXLEVEL 30
#define LINESFORLVLUP 10


int linux_console();


int linux_console(void)
{
    struct termios oldt, newt;
    int ch;

    tcgetattr( STDIN_FILENO, &oldt );
    newt = oldt;

    newt.c_lflag &= ~( ICANON | ECHO );
    tcsetattr( STDIN_FILENO, TCSANOW, &newt );

    ch = getchar();

    tcsetattr( STDIN_FILENO, TCSANOW, &oldt );

    return ch;
}


void shoe_next();
void updatescrn();
void updatescore();
void toplist();
void addscore();
int  gameover();
void checkclr();
void initpiece();
void rotate();
void moveleft();
void moveright();
int movedown();
void init();
void updatelevel();
void setkeybint();
int game();
int Enter;

void show_logo();


void concept_1();
void concept_2();
void concept_3();
void concept_4();
int  Key_setting();

struct timeval t1, t2;

char TetrominoI[2][WIDTH] = {"<! . . . . . . . . . .!>",
                             "<! . . .[][][][] . . .!>"};

char TetrominoJ[2][WIDTH] = {"<! . . .[] . . . . . .!>",
                             "<! . . .[][][] . . . .!>"};

char TetrominoL[2][WIDTH] = {"<! . . . . .[] . . . .!>",
                             "<! . . .[][][] . . . .!>"};

char TetrominoO[2][WIDTH] = {"<! . . . .[][] . . . .!>",
                             "<! . . . .[][] . . . .!>"};

char TetrominoS[2][WIDTH] = {"<! . . . .[][] . . . .!>",
                             "<! . . .[][] . . . . .!>"};

char TetrominoT[2][WIDTH] = {"<! . . . .[] . . . . .!>",
							 "<! . . .[][][] . . . .!>"};

char TetrominoZ[2][WIDTH] = {"<! . . .[][] . . . . .!>",
                             "<! . . . .[][] . . . .!>"};

#endif

```  


리눅스에서는 그래픽작업 헤어와 특수 문자 활요이 되지 않기 때문에 아래의 소스코드처럼 하나씩 이동시키는 로직으로 구현을 하였습니다.  
4차원 배열을 사용하면 가독성이 훨씬 좋아지지만 배열 연산을 공부하기 위해서 2차원으로 본 소스코드를 구현하였습니다.  

추후에 3차원과 4차원 배열을 활용하여서 소스코드를 활용할 생각입니다. 

```c
#include "program.h"

void moveleft(){
  switch(piece){
    case 'I':
      if(center[fixedpoint[0]][fixedpoint[1]+-6]=='['
         || center[fixedpoint[0]][fixedpoint[1]-6]=='<') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-6,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      fixedpoint[1]-=2;
      return;
    case 'i':
    if(center[fixedpoint[0]][fixedpoint[1]-2]=='<') return;
      for(int i=-2; i<2; ++i){
        if(center[fixedpoint[0]+i][fixedpoint[1]-2]=='[') return;
      }
      for(int i=-2; i<2; ++i){
        memcpy(center[fixedpoint[0]+i]+fixedpoint[1]-2,
               TetrominoT[0]+10, 4);
      }
      fixedpoint[1]-=2;
      return;
    case 'J':
      if(center[fixedpoint[0]][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]-4]=='[') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             TetrominoI[0]+fixedpoint[1]-2, 2);
      fixedpoint[1]-=2;
      return;
    case 'K':
      if(center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]][fixedpoint[1]-2]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      fixedpoint[1]-=2;
      return;
    case 'j':
      if(center[fixedpoint[0]][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]+1][fixedpoint[1]]=='[') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1],
             TetrominoT[0]+10, 4);
      fixedpoint[1]-=2;
      return;
    case 'k':
      if(center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-4]=='[') return;
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1]-2, 2);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1],
             TetrominoI[0]+fixedpoint[1], 2);
      fixedpoint[1]-=2;
      return;
    case 'L':
      if(center[fixedpoint[0]][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]]=='[') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1],
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      fixedpoint[1]-=2;
      return;
    case 'M':
      if(center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]][fixedpoint[1]-2]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 2);
      fixedpoint[1]-=2;
      return;
    case 'l':
      if(center[fixedpoint[0]][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]+1][fixedpoint[1]-4]=='[') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-4,
             TetrominoT[0]+10, 4);
      fixedpoint[1]-=2;
      return;
    case 'm':
      if(center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]-1][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]+1][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]-1][fixedpoint[1]-4]=='[') return;
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1]-2, 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1],
             TetrominoI[0]+fixedpoint[1], 2);
      fixedpoint[1]-=2;
      return;
    case 'O':
      if(center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]][fixedpoint[1]-2]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             center[fixedpoint[0]-1]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      fixedpoint[1]-=2;
      return;
    case 'S':
      if(center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-4]=='<') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-4,
             center[fixedpoint[0]+1]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1],
             TetrominoI[0]+fixedpoint[1], 2);
      fixedpoint[1]-=2;
      return;
    case 's':
      if(center[fixedpoint[0]-1][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]-1][fixedpoint[1]-2]=='<'
         || center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]]=='[') return;
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             TetrominoO[0]+10, 6);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1],
             TetrominoT[0]+10, 4);
      fixedpoint[1]-=2;
      return;
    case 'T':
      if(center[fixedpoint[0]][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1],
             TetrominoI[0]+fixedpoint[1], 2);
      fixedpoint[1]-=2;
      return;
    case 'U':
      if(center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]][fixedpoint[1]-2]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             TetrominoO[0]+10, 6);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      fixedpoint[1]-=2;
      return;
    case 't':
      if(center[fixedpoint[0]][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]+1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-4,
             TetrominoT[0]+10, 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      fixedpoint[1]-=2;
      return;
    case 'u':
      if(center[fixedpoint[0]][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]-1][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-4,
             TetrominoO[0]+10, 6);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      fixedpoint[1]-=2;
      return;
    case 'Z':
      if(center[fixedpoint[0]][fixedpoint[1]-4]=='['
         || center[fixedpoint[0]][fixedpoint[1]-4]=='<'
         || center[fixedpoint[0]+1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-4,
             center[fixedpoint[0]]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]]+fixedpoint[1],
             TetrominoI[0]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-2,
             center[fixedpoint[0]+1]+fixedpoint[1], 2);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]+2,
             TetrominoI[0]+fixedpoint[1]+2, 2);
      fixedpoint[1]-=2;
      return;
    case 'z':
      if(center[fixedpoint[0]-1][fixedpoint[1]]=='['
         || center[fixedpoint[0]][fixedpoint[1]-2]=='<'
         || center[fixedpoint[0]][fixedpoint[1]-2]=='['
         || center[fixedpoint[0]+1][fixedpoint[1]-2]=='[') return;
      memcpy(center[fixedpoint[0]-1]+fixedpoint[1],
             TetrominoT[0]+10, 4);
      memcpy(center[fixedpoint[0]]+fixedpoint[1]-2,
             TetrominoO[0]+10, 6);
      memcpy(center[fixedpoint[0]+1]+fixedpoint[1]-2,
             TetrominoT[0]+10, 4);
      fixedpoint[1]-=2;
      return;
  }
}
```  
