---

title:  "임베디드 국비과정반 첫번째 실기시험 과제 제출 포스팅"
excerpt: "C programming_basic codding"
header:

categories:
  - C/C++
tags:
  - C/C++
---
  
  
**대한상공회의소 임베디드 기반 IoT SW전문가 양성반의 첫번째 실기시험 제출 과제코드입니다.**   

**How to Run This build**
```c
$ compile.sh
```

**Shell Script File**
```c
#!/bin/bash

#####################################################################
#Script Name	: compile.sh                                        #
#Description	: 2021.06.09                                        #
#Args			:                                                   #
#Author			:                                                   #
#Email			: jeewoo19930315@gmail.com                          #
#Blog			: azabell.github.io                                 #
#####################################################################

catch_name()
{
	echo -n "USER NAME : "
	whoami > whoami.txt
	cat whoami.txt
	cat whoami.txt >> whoami_saved.txt
}

run_main()
{
	echo "-------------------------------------------------------------\n"
	echo "만나서 반갑습니다."
	echo "-------------------------------------------------------------\n"
	echo "make 명령어를 자동실행합니다. -------------------------------\n"
	echo "-------------------------------------------------------------\n"
	make
	echo " ------------------------------------------------------------\n"
	echo " gcc -o main main.c error_functions.o를 자동실행----\n"
	echo " ------------------------------------------------------------\n"
	gcc -o main main.c error_functions.o
	echo " ------------------------------------------------------------\n"
	echo " -------------이제 정보를 기입하세요.------------------------\n"
	echo " ------------------------------------------------------------\n"

	./main


	echo "-------------------------------------------------------------\n"
	echo "------------------저장된 정보를 보여드립니다.----------------\n"
	cat whoami_saved.txt
	echo "-------------------------------------------------------------\n"
	echo "-------------text file입니다.--------------------------------\n"
	cat notepad.txt
	echo "-------------------------------------------------------------\n"
	echo "------------------저장된 정보를 로그에 저장시킵니다.---------\n"
	cat notepad.txt >> Log.txt
	echo "-------------------------------------------------------------\n"
	echo "------------------Log file 정보를 열람합니다.----------------\n"
	cat Log.txt

	echo "=============================================================\n"
	echo "makef file로 생성된 잔여 파일들을 자동삭제합니다. \n"
	echo "=============================================================\n"

	make clean
}

catch_name
run_main
```

  
직접 만들어둔 리눅스 **API**(C언어 프로그래밍 파일)을 이용하여서 파일 스트림 함수에 errExit()이 실행이 가능하도록 Makefile을 직접 구현하였습니다.
그리고 쉘 스크립트 자동화로 프로그램을 실행하고 컴퓨터 사용자의 이름과 성적까지 로그파일로 저장이 되게 프로그래밍을 하였습니다.

단독 C언어 프로그램 실행만으로는 저장이 한 번만 되도록 하였습니다.

아래는 리눅스 자동화 쉘 스크립트로 만들어진 파일과 구현 모습입니다.

![Screenshot from 2021-06-09 21-00-43](https://user-images.githubusercontent.com/75885992/121351705-081f1000-c967-11eb-8b3b-7fab32891524.png)
![Screenshot from 2021-06-09 21-01-00](https://user-images.githubusercontent.com/75885992/121351710-09503d00-c967-11eb-9ccb-ecae089f3086.png)

![Screenshot from 2021-06-09 21-09-49](https://user-images.githubusercontent.com/75885992/121351814-21c05780-c967-11eb-9af8-9879f2997328.png)

**반복하여서 build를 하고 정보를 추가로 입력한다면 Log파일에 내역이 추가가 됩니다.**

![Screenshot from 2021-06-09 21-13-50](https://user-images.githubusercontent.com/75885992/121352402-c9d62080-c967-11eb-97f3-50c14dde47d9.png)
![Screenshot from 2021-06-09 21-14-02](https://user-images.githubusercontent.com/75885992/121352404-cb074d80-c967-11eb-9c36-0e1b5a7d6311.png)


  
  
먼저 헤더파일을 만들어주고 생성을 해줬습니다.  
아주 재미있는 셋팅 놀이지요.  
  
```c
#ifndef PROGRAM_H_
#define PROGRAM_H_

#define _CRT_SECURE_NO_WARININGS

#define C_NRML "\033[0m"
#define C_BLCK "\033[30m"
#define C_RED  "\033[31m"
#define C_GREN "\033[32m"
#define C_YLLW "\033[33m"
#define C_BLUE "\033[34m"
#define C_PRPL "\033[35m"
#define C_AQUA "\033[36m"

#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>

/*INPUT LINUX API*/
#include "error_functions.h"
#include "get_num.h"

#define MAX_STUDENT_COUNT 10
#define MAX_NAME_LENGTH   10
#define PART_COUNT 5
#define PART_DATA_SIZE sizeof(int)*PART_COUNT

#define KOR 0
#define ENG 1
#define MATH 2
#define SIENCE 3
#define ETHICS 4

int i;
int j;


int TOTAL[101];
int TOTAL_SAVE[101];
float AVERAGE[101];
int LANK[101];

void Swap_Score(int score_first[], int score_second[]);
void Swap_Name(char name_first[], char name_second[]);
void Bubble_Sort(char name[][MAX_NAME_LENGTH], int score[][PART_COUNT]);
void Input_Result(char name[][MAX_NAME_LENGTH], int score[][PART_COUNT]);
void Result_scv(char name[][MAX_NAME_LENGTH], int score[][PART_COUNT]);
void Lank();

FILE *fp;
FILE *tfp;

#endif
```  

**main.c 파일입니다. 프로그램 구동의 중심이 되는 심장을 코딩했습니다.**    
  
```c
#include "program.h"
#include <unistd.h>

void ft_putchar(char c)
{
	write(1, &c, 1);
}


void Swap_Score(int score_first[], int score_second[])
{
	int temp[PART_COUNT];

	memcpy(temp, score_first, PART_DATA_SIZE);
	memcpy(score_first, score_second, PART_DATA_SIZE);
	memcpy(score_second, temp, PART_DATA_SIZE);
}



void Swap_Name(char name_first[], char name_second[])
{
	char temp_name[MAX_NAME_LENGTH];

	strcpy(temp_name, name_first);
	strcpy(name_first, name_second);
	strcpy(name_second, temp_name);
}



void Bubble_Sort(char name[][MAX_NAME_LENGTH], int score[][PART_COUNT])
{
	for(i=0; i<MAX_STUDENT_COUNT-1; i++)
	{
		for(j=0; j<MAX_STUDENT_COUNT-1-i; j++)
		{
			if(TOTAL[j] > TOTAL[j+1])
			{
				Swap_Name(name[j], name[j+1]);
				Swap_Score(score[j], score[j+1]);
			}
		}
	}
}


void InputData(char name[][MAX_NAME_LENGTH], int score[][PART_COUNT])
{
	for(i=0; i<MAX_STUDENT_COUNT; i++)
	{
		LANK[i]=1;

		printf("%s %d 번째 학생 이름 : ", C_YLLW ,i+1); 	scanf("%s", name+i);
		printf("%s 국어 점수 : ", C_BLUE);				 	scanf("%d", score[i] + KOR);
		printf("%s 영어 점수 : ", C_BLUE);				 	scanf("%d", score[i] + ENG);
		printf("%s 수학 점수 : ", C_BLUE);				 	scanf("%d", score[i] + MATH);
		printf("%s 과학 점수 : ", C_BLUE); 			 		scanf("%d", score[i] + SIENCE);
		printf("%s 윤리 점수 : ", C_BLUE); 			 		scanf("%d", score[i] + ETHICS);
		ft_putchar('\n');
	}

	for(i=0; i<MAX_STUDENT_COUNT; i++)
	{
		TOTAL[i] = score[i][KOR] + score[i][ENG] + score[i][MATH] + score[i][SIENCE] + score[i][ETHICS];
		AVERAGE[i] = TOTAL[i]/PART_COUNT;
	}

	Lank();
}

void Lank()
{
	for(j=0; j<MAX_STUDENT_COUNT; j++)
	{
		for(i=0; i<MAX_STUDENT_COUNT; i++)
		{
			if(TOTAL[i] != TOTAL[j] && TOTAL[i] < TOTAL[j])
			{
				LANK[i]++;
			}
		}
	}
}

void Display_Result(char name[][MAX_NAME_LENGTH], int score[][PART_COUNT])
{
	fprintf(tfp,"%s 학번, 성명, 국어, 영어, 수학, 과학, 윤리, 총점, 평균, 석차\n", C_PRPL);

	for(i=0; i<MAX_STUDENT_COUNT; i++){
		fprintf(tfp,"%2d, %6s,  %3d,  %3d,  %3d,  %3d,  %3d,  %3d, %.2f,%3d등\n",
				i+1, name[i], score[i][KOR], score[i][ENG], score[i][MATH], score[i][SIENCE],
				score[i][ETHICS], TOTAL[i], AVERAGE[i], LANK[i]);
	}
}


void Result_scv(char name[][MAX_NAME_LENGTH], int score[][PART_COUNT])
{
	fprintf(fp,"%s 학번, 성명, 국어, 영어, 수학, 과학, 윤리, 총점, 평균, 석차\n", C_PRPL);

	for(i=0; i<MAX_STUDENT_COUNT; i++){
		fprintf(fp,"%2d, %6s,  %3d,  %3d,  %3d,  %3d,  %3d,  %3d, %.2f,%3d등\n",
				i+1, name[i], score[i][KOR], score[i][ENG], score[i][MATH], score[i][SIENCE],
				score[i][ETHICS], TOTAL[i], AVERAGE[i], LANK[i]);
	}
}

int main(int argc, char *argv[])
{
	char name[MAX_STUDENT_COUNT][MAX_NAME_LENGTH];
	int score[MAX_STUDENT_COUNT][PART_COUNT];

	InputData(name, score);
	//Bubble_Sort(name, score);

	if((tfp=fopen("notepad.txt","wt")) == NULL)
	{
		errExit("Text File open error\n");
	}
	else
	{
		fprintf(tfp,"Text file_Input complete!!\n");
	}
	Display_Result(name,score);


	if((fp=fopen("excell.csv","wt")) == NULL)
	{
		errExit("File open error\n");
	}
	else
	{
		fprintf(fp,"Excell File_Input complete!!\n");
	}

	Result_scv(name,score);
	fclose(tfp);
	fclose(fp);

	char buffer[101];

	printf("%s 저장된 정보를 보여드리겠습니다.\n", C_BLUE);

	printf("%s =====================================================================\n", C_AQUA);
	printf("                          텍스트 파일입니다.                         \n");
	printf("=====================================================================\n");

	if ((tfp = fopen("notepad.txt", "rt")) != NULL)
	{
		memset(buffer, 0, sizeof(buffer));
		while (fread(buffer, 10, 256, tfp) != 0)
		printf("%s\n", buffer);
		fclose(tfp);
	}
	else
	{
		printf("파일 오픈 에러!!!\n");
	}

	printf("%s 성공적으로 파일이 저장되었습니다. 리눅스라면 cat notepad.txt로 확인하세요.\n", C_BLUE);
	ft_putchar('\n');

	printf("%s =====================================================================\n", C_AQUA);
	printf("                           엑셀 파일 입니다.                         \n");
	printf("=====================================================================\n");

	if ((fp = fopen("excell.csv", "rt")) != NULL)
	{
    	memset(buffer, 0, sizeof(buffer));
    	while (fread(buffer, 10, 256, fp) != 0)
       	printf("%s\n", buffer);
    		fclose(fp);
	}
	else
	{
		printf("파일 오픈 에러!!!\n");
	}


	printf("%s 성공적으로 파일이 저장되었습니다. 리눅스라면 cat excell.scv로 확인하세요.\n", C_BLUE);

	return (0);
}

```  
  
  
  
아무래도 국비학원의 과제 난이도는 매우 쉽기 때문에 코드자체가 별로 어렵지 않고 단조롭습니다.  
그래서 많이 심심하였기 때문에  리눅스의 API 기능들을 집어넣고 Makefile까지 활용하여서 쉘 스크립트 작성후 자동화를 진행했습니다.  
api소스는 리눅스 프로그래밍 책의 저자를 참조했습니다.  
몇 번 갖다 썻다가 구축을 해보면 바로 감이 옵니다.  
  
  
  
**get_num.h**  

```c
#ifndef GET_NUM_H
#define GET_NUM_H

#define GN_NONNEG 01
#define GN_GT_0

#define GN_ANY_BASE 0100
#define GN_BASE_8  0200
#define GN_BASE_16 0400

long getLong(const char *arg, int flages, const char *name);

int getlnt(const char *arg, int flages, const char *name);

#endif
```  
  
**ename.c.inc**  
  
```c
 static char *ename[] = {
     /*   0 */ "",
     /*   1 */ "EPERM", "ENOENT", "ESRCH", "EINTR", "EIO", "ENXIO",
     /*   7 */ "E2BIG", "ENOEXEC", "EBADF", "ECHILD",
     /*  11 */ "EAGAIN/EWOULDBLOCK", "ENOMEM", "EACCES", "EFAULT",
     /*  15 */ "ENOTBLK", "EBUSY", "EEXIST", "EXDEV", "ENODEV",
     /*  20 */ "ENOTDIR", "EISDIR", "EINVAL", "ENFILE", "EMFILE",
     /*  25 */ "ENOTTY", "ETXTBSY", "EFBIG", "ENOSPC", "ESPIPE",
     /*  30 */ "EROFS", "EMLINK", "EPIPE", "EDOM", "ERANGE",
     /*  35 */ "EDEADLK/EDEADLOCK", "ENAMETOOLONG", "ENOLCK", "ENOSYS",
     /*  39 */ "ENOTEMPTY", "ELOOP", "", "ENOMSG", "EIDRM", "ECHRNG",
     /*  45 */ "EL2NSYNC", "EL3HLT", "EL3RST", "ELNRNG", "EUNATCH",
     /*  50 */ "ENOCSI", "EL2HLT", "EBADE", "EBADR", "EXFULL", "ENOANO",
     /*  56 */ "EBADRQC", "EBADSLT", "", "EBFONT", "ENOSTR", "ENODATA",
     /*  62 */ "ETIME", "ENOSR", "ENONET", "ENOPKG", "EREMOTE",
     /*  67 */ "ENOLINK", "EADV", "ESRMNT", "ECOMM", "EPROTO",
     /*  72 */ "EMULTIHOP", "EDOTDOT", "EBADMSG", "EOVERFLOW",
     /*  76 */ "ENOTUNIQ", "EBADFD", "EREMCHG", "ELIBACC", "ELIBBAD",
     /*  81 */ "ELIBSCN", "ELIBMAX", "ELIBEXEC", "EILSEQ", "ERESTART",
     /*  86 */ "ESTRPIPE", "EUSERS", "ENOTSOCK", "EDESTADDRREQ",
     /*  90 */ "EMSGSIZE", "EPROTOTYPE", "ENOPROTOOPT",
     /*  93 */ "EPROTONOSUPPORT", "ESOCKTNOSUPPORT",
     /*  95 */ "EOPNOTSUPP/ENOTSUP", "EPFNOSUPPORT", "EAFNOSUPPORT",
     /*  98 */ "EADDRINUSE", "EADDRNOTAVAIL", "ENETDOWN", "ENETUNREACH",
     /* 102 */ "ENETRESET", "ECONNABORTED", "ECONNRESET", "ENOBUFS",
     /* 106 */ "EISCONN", "ENOTCONN", "ESHUTDOWN", "ETOOMANYREFS",
     /* 110 */ "ETIMEDOUT", "ECONNREFUSED", "EHOSTDOWN", "EHOSTUNREACH",
     /* 114 */ "EALREADY", "EINPROGRESS", "ESTALE", "EUCLEAN",
     /* 118 */ "ENOTNAM", "ENAVAIL", "EISNAM", "EREMOTEIO", "EDQUOT",
     /* 123 */ "ENOMEDIUM", "EMEDIUMTYPE", "ECANCELED", "ENOKEY",
     /* 127 */ "EKEYEXPIRED", "EKEYREVOKED", "EKEYREJECTED",
     /* 130 */ "EOWNERDEAD", "ENOTRECOVERABLE", "ERFKILL", "EHWPOISON"
 };

 #define MAX_ENAME 133
```  


**tpli_hdr.h**  
  
```c
#ifndef TLPI_HDR_H
#define TLPI_HDR_H

#include <sys/types.h>
#include <stdio.h>
#include <stdlib.h>

#include <unistd.h>
#include <errno.h>
#include <string.h>

#include "get_num.h"

#include "error_functions.h"

#ifdef TRUE
#undef TRUE
#endif

#ifdef FALSE
#undef FALSE
#endif

typedef enum { FALSE, TRUE } Boolean;

#define min(m,n) ((m) < (n) ? (m):(n))
#define max(m,n) ((m) > (n) ? (m):(n))

#if defined(__sgi)
typedef int socklen_t;
#endif

#if defined(__sun)
#include <sys/file.h>
#endif

#if ! defined(O_ASYNC) && defined(FASYNC)
#define O_ASYNC FASYNC
#endif

#if defined(MAP_ANON) && ! defined(MAP_ANONVMOUS)
#define MAP_ANOVMOUS MAP_ANON

#endif

#if ! defined(O_SYNC) && defined(O_FSYNC)
#define O_SYNC O_FSYNC
#endif

#if defined(__FreeBSD__)

#define sival_int sigval_int
#define sival_ptr sigval_ptr
#endif

#endif
```  
  
  
**error_functions.h**  
  
```c

#ifndef ERROR_FUNCTIONS_H
#define ERROR_FUNCTIONS_H

void errMsg(const char *format, ...);

#ifdef _GUNC_

#define NORETURN _attribute_ ((_noreturn_))
#else
#define NORETURN
#endif

void errExit(const char *format, ...) NORETURN ;
void err_exit(const char *format, ...) NORETURN ;
void errExitEN(int errnum, const char *format, ...) NORETURN ;
void fatal(const char *format, ...) NORETURN ;
void usageErr(const char *format, ...) NORETURN ;
void cmdLineErr(const char *format, ...) NORETURN ;

#endif
```  
  
  
**error_functions.c**  
  

```c
#include <stdarg.h>
#include "error_functions.h"
#include "tlpi_hdr.h"
#include "ename.c.inc"

#ifdef _GNUC_
_attribute_ ((_noreturn_))
#endif

static void terminate(Boolean useExit3)
{
	char *s;
	s = getenv("EF_DUMPCORE");

	if (s != NULL && *s != '\0')
		abort();
	else if (useExit3)
		exit(EXIT_FAILURE);
	else
		_exit(EXIT_FAILURE);
}

static void outputError(Boolean useErr, int err, Boolean flushStdout, const char *format, va_list ap)
{
	#define BUF_SIZE 500
		char buf[BUF_SIZE], userMsg[BUF_SIZE], errText[BUF_SIZE];
		vsnprintf(userMsg, BUF_SIZE, format, ap);

		if (useErr)
			snprintf(errText, BUF_SIZE, " [%s %s]",
					(err > 0 && err <= MAX_ENAME) ? ename[err] : "?UNKOWN?", strerror(err));
		else
				snprintf(errText, BUF_SIZE, ":");

		snprintf(buf, BUF_SIZE, "ERROR%s %s \n", errText, userMsg);

		if (flushStdout)
			fflush(stdout);
		fputs(buf, stderr);
		fflush(stderr);
}


void	errMsg(const char *format, ...)
{
	va_list argList;
	int savedErrno;

	savedErrno = errno;

	va_start(argList, format);
	outputError(TRUE, errno, TRUE, format, argList);
	va_end(argList);

	errno = savedErrno;
}

void	errExit(const char *format, ...)
{
	va_list argList;

	va_start(argList, format);
	outputError(TRUE, errno, TRUE, format, argList);
	va_end(argList);

	terminate(TRUE);
}

void	err_exit(const char *format, ...)
{
	va_list argList;

	va_start(argList, format);
	outputError(TRUE, errno, FALSE, format, argList);
	va_end(argList);

	terminate(FALSE);
}

void	errExitEN(int errnum, const char *format, ...)
{
	va_list argList;

	va_start(argList, format);
	outputError(TRUE, errnum, TRUE, format, argList);
	va_end(argList);

	terminate(TRUE);
}

void	fatal(const char *format, ...)
{
	va_list argList;

	va_start(argList, format);
	outputError(FALSE, 0, TRUE, format, argList);
	va_end(argList);

	terminate(TRUE);
}

void	usageErr(const char *format, ...)
{
	va_list argList;

	fflush(stdout);

	fprintf(stderr, "Usage: ");
	va_start(argList, format);
	vfprintf(stderr, format, argList);
	va_end(argList);

	fflush(stderr);
	exit(EXIT_FAILURE);
}

void	cmdLineErr(const char *format, ...)
{
	va_list argList;

	fflush(stdout);

	fprintf(stderr, "Command-line usage error: ");
	va_start(argList, format);
	vfprintf(stderr, format, argList);
	va_end(argList);

	fflush(stderr);
	exit(EXIT_FAILURE);
}
```  

이제 기본적인 준비물들은 모두 구성이 완료가 되었습니다.  
본격적으로 자동화를 위한 간단한 리눅스 명령어 문법을 활용하여서 쉘 스크립트 자동화를 시켜줍니다.  
인터넷에 연결해주기 위해서 자바스크립트 코딩을 할 필요는 없었으므로 아주 기초적인 명령어를 활용하였기 때문에 코드는 쉽습니다.  
  

파일의 네이밍을 빌드할 때 찾아보기 쉽도록 간단히 compile.sh로 진행하였습니다.  
**compile.sh**  
  
```c
#!/bin/bash

#####################################################################
#Script Name	: compile.sh                                        #
#Description	: 2021.06.09                                        #
#Args			:                                                   #
#Author			:                                                   #
#Email			: jeewoo19930315@gmail.com                          #
#Blog			: azabell.github.io                                 #
#####################################################################

catch_name()
{
	echo -n "USER NAME : "
	whoami > whoami.txt
	cat whoami.txt
	cat whoami.txt >> whoami_saved.txt
}

run_main()
{
	echo "-------------------------------------------------------------\n"
	echo "만나서 반갑습니다."
	echo "-------------------------------------------------------------\n"
	echo "make 명령어를 자동실행합니다. -------------------------------\n"
	echo "-------------------------------------------------------------\n"
	make
	echo " ------------------------------------------------------------\n"
	echo " gcc -o main main.c error_functions.o를 자동실행----\n"
	echo " ------------------------------------------------------------\n"
	gcc -o main main.c error_functions.o
	echo " ------------------------------------------------------------\n"
	echo " -------------이제 정보를 기입하세요.------------------------\n"
	echo " ------------------------------------------------------------\n"

	./main


	echo "-------------------------------------------------------------\n"
	echo "------------------저장된 정보를 보여드립니다.----------------\n"
	cat whoami_saved.txt
	echo "-------------------------------------------------------------\n"
	echo "-------------text file입니다.--------------------------------\n"
	cat notepad.txt
	echo "-------------------------------------------------------------\n"
	echo "------------------저장된 정보를 로그에 저장시킵니다.---------\n"
	cat notepad.txt >> Log.txt
	echo "-------------------------------------------------------------\n"
	echo "------------------Log file 정보를 열람합니다.----------------\n"
	cat Log.txt

	echo "=============================================================\n"
	echo "makef file로 생성된 잔여 파일들을 자동삭제합니다. \n"
	echo "=============================================================\n"

	make clean
}

catch_name
run_main
```  
  
자동화를 진행한다면 몇가지 파일들이 생성됩니다.  
```c
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  #가장 먼저 저장이 되는 텍스트 파일입니다.
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  cat notepad.txt
Text file_Input complete!!
 학번, 성명, 국어, 영어, 수학, 과학, 윤리, 총점, 평균, 석차
 1, 박지우,   60,   50,   40,   15,   35,  200, 40.00,  9등
 2, 송슬기,   70,   65,   44,   15,   48,  242, 48.00,  4등
 3, 김태희,   50,   60,   70,   44,   58,  282, 56.00,  2등
 4, 장동건,   45,   68,   45,   16,   68,  242, 48.00,  4등
 5, 유재하,   40,   85,   14,   35,   44,  218, 43.00,  7등
 6, 아이유,   78,   45,   16,   68,   11,  218, 43.00,  7등
 7, 유지태,   44,   78,   98,   65,   19,  304, 60.00,  1등
 8, 김슬기,   49,   68,   49,   33,   45,  244, 48.00,  3등
 9, 혜리짱,   45,   19,   65,   11,   38,  178, 35.00, 10등
10, 류준얼,   55,   45,   16,   68,   46,  230, 46.00,  6등
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  #엑셀 파일도 저장을 하였습니다. 아치리눅스에서 열기 힘들테지만 우분투에서는 deb패키지가 잘되어있어서 걱정이 없습니다.
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  cat excell.csv
Excell File_Input complete!!
 학번, 성명, 국어, 영어, 수학, 과학, 윤리, 총점, 평균, 석차
 1, 박지우,   60,   50,   40,   15,   35,  200, 40.00,  9등
 2, 송슬기,   70,   65,   44,   15,   48,  242, 48.00,  4등
 3, 김태희,   50,   60,   70,   44,   58,  282, 56.00,  2등
 4, 장동건,   45,   68,   45,   16,   68,  242, 48.00,  4등
 5, 유재하,   40,   85,   14,   35,   44,  218, 43.00,  7등
 6, 아이유,   78,   45,   16,   68,   11,  218, 43.00,  7등
 7, 유지태,   44,   78,   98,   65,   19,  304, 60.00,  1등
 8, 김슬기,   49,   68,   49,   33,   45,  244, 48.00,  3등
 9, 혜리짱,   45,   19,   65,   11,   38,  178, 35.00, 10등
10, 류준얼,   55,   45,   16,   68,   46,  230, 46.00,  6등
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  #자동화를 진행시킬 쉘 스크립트 코드에 로그로 내역들을 저장시키는 코드를 넣었기 때문에 로그도 나옵니다.
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  cat Log.txt
Text file_Input complete!!
 학번, 성명, 국어, 영어, 수학, 과학, 윤리, 총점, 평균, 석차
 1, 홍길동,   90,   60,   50,   80,   70,  350, 70.00,  4등
 2, 민경미,   50,   80,   70,   50,   50,  300, 60.00,  8등
 3, 김기성,   70,   90,   90,   60,   30,  340, 68.00,  5등
 4, 박지훈,   90,   70,   50,   90,   60,  360, 72.00,  2등
 5, 손흥민,   50,   50,   30,   50,   60,  240, 48.00, 10등
 6, 류현진,   30,   70,   60,   30,   80,  270, 54.00,  9등
 7, 강동원,   60,   90,   60,   60,   90,  360, 72.00,  2등
 8, 동물원,   60,   50,   80,   60,   60,  310, 62.00,  7등
 9, 유재하,   80,   30,   50,   80,   80,  320, 64.00,  6등
10, 김광석,   90,   60,   80,   90,   90,  410, 82.00,  1등
Text file_Input complete!!
 학번, 성명, 국어, 영어, 수학, 과학, 윤리, 총점, 평균, 석차
 1, 박지우,   60,   50,   40,   15,   35,  200, 40.00,  9등
 2, 송슬기,   70,   65,   44,   15,   48,  242, 48.00,  4등
 3, 김태희,   50,   60,   70,   44,   58,  282, 56.00,  2등
 4, 장동건,   45,   68,   45,   16,   68,  242, 48.00,  4등
 5, 유재하,   40,   85,   14,   35,   44,  218, 43.00,  7등
 6, 아이유,   78,   45,   16,   68,   11,  218, 43.00,  7등
 7, 유지태,   44,   78,   98,   65,   19,  304, 60.00,  1등
 8, 김슬기,   49,   68,   49,   33,   45,  244, 48.00,  3등
 9, 혜리짱,   45,   19,   65,   11,   38,  178, 35.00, 10등
10, 류준얼,   55,   45,   16,   68,   46,  230, 46.00,  6등
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  #쉘의 코드를 보면 컴퓨터 사용자의 이름을 나오게 설정도 했습니다.
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  cat whoami.txt
azabell
 azabell@azabellLaptop  ~/Desktop/sumbit_subject/Sub_first_Makefile   main ±  cat whoami_saved.txt
azabell
azabell
```  
  
  
이대로 끝내면 너무나 아쉽습니다.  
그래서 분산과 표준편차까지 넣어줬습니다.  
간단하게 총 합계만 구하고 pow를 이용한다면 단 3줄로 정리가 끝나지만 각각의 분산과 표준편차를 구하는 코드를 직접 내장함수로 구현하고자 하였고, 코드를 길게한 것과 짧게 축약한 것 버전을 나눠서  
올려보겠습니다.  
   
작은 기능부터 하나씩 구현해야 코드 구현이 좋아집니다.  
  
**축약하지 않은 버전**  
  
```c
double variance_0;
double variance_1;
double variance_2;
double variance_3;
double variance_4;
double variance_5;
double variance_6;
double variance_6;
double variance_7;
double variance_8;
double variance_9;

double std)deviation_0;
double std)deviation_1;
double std)deviation_2;
double std)deviation_3;
double std)deviation_4;
double std)deviation_5;
double std)deviation_6;
double std)deviation_7;
double std)deviation_8;
double std)deviation_9;

void Result(int score[][PART])
{
	variance_0 = variance+_0 + (score[0][i] - AVERAGE[0]) * (score[0][i] - AVERAGE[0]);
	variance_1 = variance+_1 + (score[1][i] - AVERAGE[1]) * (score[1][i] - AVERAGE[1]);
	variance_0 = variance+_0 + (score[2][i] - AVERAGE[2]) * (score[2][i] - AVERAGE[2]);
	variance_0 = variance+_0 + (score[3][i] - AVERAGE[3]) * (score[3][i] - AVERAGE[3]);
	variance_0 = variance+_0 + (score[4][i] - AVERAGE[4]) * (score[4][i] - AVERAGE[4]);
	variance_0 = variance+_0 + (score[5][i] - AVERAGE[5]) * (score[5][i] - AVERAGE[5]);
	variance_0 = variance+_0 + (score[6][i] - AVERAGE[6]) * (score[6][i] - AVERAGE[6]);
	variance_0 = variance+_0 + (score[7][i] - AVERAGE[7]) * (score[7][i] - AVERAGE[7]);
	variance_0 = variance+_0 + (score[8][i] - AVERAGE[8]) * (score[8][i] - AVERAGE[8]);
}

variance_0 = variancd_0 / PART_COUNT;
variance_0 = variancd_0 / PART_COUNT;
variance_0 = variancd_0 / PART_COUNT;
variance_0 = variancd_0 / PART_COUNT;
variance_0 = variancd_0 / PART_COUNT;
variance_0 = variancd_0 / PART_COUNT;
variance_0 = variancd_0 / PART_COUNT;
variance_0 = variancd_0 / PART_COUNT;
variance_0 = variancd_0 / PART_COUNT;

std_deviation_0 = sqrt(variance_0);
std_deviation_1 = sqrt(variance_1);
std_deviation_2 = sqrt(variance_2);
std_deviation_3 = sqrt(variance_3);
std_deviation_4 = sqrt(variance_4);
std_deviation_5 = sqrt(variance_5);
std_deviation_6 = sqrt(variance_6);
std_deviation_7 = sqrt(variance_7);
std_deviation_8 = sqrt(variance_8);
std_deviation_9 = sqrt(variance_9);
}
```  

하나씩 일일이 해주는 방법이 있습니다.  
이렇게 해주고 main으로 돌아가서  printf함수에 대입을 해주면 됩니다.  
  
다음은 쉽게 구성한 축약버전입니다.  
  
```c
double variance;
double std_deviation;

double variance_2;
double std_deviation_2;

/*text file stream setting*/
void Loop_Result(int score[][PART_COUNT])
{
	for(int i=0; i<MAX_STUDENT_COUNT; i++)
	{
		variance = 0;
		variance = variance + (
			(((score[i][0] - AVERAGE[i]) * ((score[i][0] - AVERAGE[i])) +
			(((score[i][1] - AVERAGE[i]) * (score[i][1] - AVERAGE[i])) +
			(((score[i][2] - AVERAGE[i]) * ((score[i][2] - AVERAGE[i])) +
			(((score[i][3] - AVERAGE[i]) * ((score[i][3] - AVERAGE[i])) +
			(((score[i][4] - AVERAGE[i]) * ((score[i][4]) - AVERAGE[i]))
			 );

		variance = variance / PART_COUNT;
		std_deviation = sqrt(variance);

		fprintf(tfp, "%d번째 분산, %.3lf, 표준 편차, %.3f\n", i+1, variance, std_deviation);
	}
}

/*excell file stream setting*/
void Loop_Result(int score[][PART_COUNT])
{
	for(int i=0; i<MAX_STUDENT_COUNT; i++)
	{
		variance_2 = 0;
		variance_2 = variance_2 + (
			(((score[i][0] - AVERAGE[i]) * ((score[i][0] - AVERAGE[i])) +
			(((score[i][1] - AVERAGE[i]) * (score[i][1] - AVERAGE[i])) +
			(((score[i][2] - AVERAGE[i]) * ((score[i][2] - AVERAGE[i])) +
			(((score[i][3] - AVERAGE[i]) * ((score[i][3] - AVERAGE[i])) +
			(((score[i][4] - AVERAGE[i]) * ((score[i][4]) - AVERAGE[i]))
			 );

		variance_2 = variance_2 / PART_COUNT;
		std_deviation_2 = sqrt(variance_2);

		fprintf(fp, "%d번째 분산, %.3lf, 표준 편차, %.3f\n", i+1, variance_2, std_deviation_2);
	}
}
```  

여기서 fprintf를 구현하였기 때문에 그냥 함수명만 가져다가 그대로 구현하면 됩니다.  
  
  
