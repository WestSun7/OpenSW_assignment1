# OpenSW_assignment1
OpenSW_assignment1

이것은 **getopt, getopts, sed, awk** 명령어에 대한 설명입니다.


* **getopt**
   * 정의: **getopt** 는 파이썬 기본 라이브러리 중 하나로 **명령행** 옵션을 처리해주는 모듈이다. **명령행**에서 입력된 옵션들을 정의하고 설정한 변수 값으로 할당해준다. 옵션 인수는 유닉스 getopt() 함수와 동일하게 – 혹은 -- 로 시작한다.
   * 원형: ``` int getopt(int argc, char * const argv[], const char *optstring); ```
   * 파라미터: argc, argv : main() 함수가 받은 파라미터를 그대로 전달한다. optstring : 파싱해야 할 파라미터를 쓴다. 옵션이 별도의 파라미터를 받는 경우 콜론을 함께 쓴다. 
   * 예를 들어 -h, -v, -f filename을 받는 세 가지 옵션이 있다고 하면 옵션스트링은 **"hvf:"**가 된다. 각각의 옵션을 파싱해내기 위해서는 getopt()함수가 0을 리턴할 때까지 계속해서 반복하면 된다.
   * 샘플 코드: 이 코드에서는 a, b, c의 세 옵션을 인식하며 각각의 옵션이 주어지는 경우 해당 플래그 변수를 1로 정의하고 그 결과를 출력한다. 
```C
1:  #include <stdio.h>
2:  #include <unistd.h> // for getopt()
3:
4:  int main(int argc, char * const * argv){
5:      int flag_a = 0, flag_b = 0, flag_c = 0;
6:      int c; // option
7:      while( (c = getopt(argc, argv, "abc")) != -1) {
8:          // -1 means getopt() parse all options
9:          switch(c) {
10:             case 'a':
11:                 flag_a=1;
12:                 break;
13:             case 'b':
14:                 flag_b=1;
15:                 break;
16:             case 'c':
17:                 flag_c=1;
18:                 break;
19:             case '?':
20:                 printf("Unknown flag : %c", optopt);
21:                 break;
22:         }
23:     }
24:     if(flag_a) {printf("flag a is ON \n");}
25:     if(flag_b) {printf("flag b is ON \n");}
26:     if(flag_c) {printf("flag c is ON \n");}
27:     return 0;
28: }
```

* **getopts**
  * 정의 : getopts는 명령줄 인수를 구문 분석하기 위한 내장 Unix 셸 명령입니다.
  * getopts가 나오게 된 배경: getopt에서 **인수의 공백** 또는 **쉘 메타 문자**를 처리할 수 없었고 **오류 메시지 출력을 비활성화**하는 기능이 없었습니다.
  * getopt와 유사한 getopts의 사용방법 ```getopt optstring [parameters] getopts optstring varname [parameters] ```
  * 샘플 코드:
```C
1:#!/bin/sh
2:while getopts ':a:l:v' opt; do
3:    case $opt in
4:      (v)   ((VERBOSE++));;
5:      (a)   ARTICLE=$OPTARG;;
6:      (l)   LANG=$OPTARG;;
7:      (:)   # "optional arguments" (missing option-argument handling)
8:            case $OPTARG in
9:              (a) exit 1;; # error, according to our syntax
10:              (l) :;;      # acceptable but does nothing
11:            esac;;
12:    esac
13:done
14:
15:shift "$OPTIND"
16:# remaining is "$@"
```

* **sed**
  * 정의: sed(stream editor)는 유닉스에서 텍스트를 **분해**하거나 **변환**하기 위한 프로그램이다.
  * 역사: 버전 7 유닉스에서 처음 등장한 sed는 데이터 파일의 명령 줄 처리를 위해 개발된 초기 유닉스 명령어들 가운데 하나입니다. 대중적인 grep 명령어의 뒤를 자연스럽게 이을 정도로 발전하였습니다.원래는 치환을 목적으로 한 grep(g/re/p)의 상이형인 "g/re/s"이었습니다. 개별 명령어를 위해 추가적인 특수 목적의 프로그램들이 등장할 것으로 예견하면서 맥마흔은 범용 목적의 라인 지향 스트림 편집기를 작성하였으며, 이것이 sed로 되었습니다.
  * 치환 명령어 : 다음의 예는 sed의 가장 일반적인 치환의 예입니다. 이 사용법은 실제로 sed의 원래 동기와 부합합니다.

     ```C
     sed 's/regexp/replacement/g' inputFileName > outputFileName 
     ```
  * 기타 sed 명령어 : 치환 외에도 25개의 sed 명령을 사용하여 다른 형태의 단순한 처리가 가능합니다. 이를테면, 다음의 경우 d 명령어를 사용하여 비어있거나 공백만 포함하는 줄을 삭제합니다.
     ```C
     sed '/^ *$/d' inputFileName
     ```
  * 필터로서의 사용 : 유닉스에서 sed는 파이프 안에 필터로 종종 사용됩니다.
     ```C
     generateData | sed 's/x/y/g'
     ```
  * 파일 기반 sed 스크립트 : 한 줄에 하나의 명령으로 여러 sed 명령을 subst.sed와 같은 스크립트 파일 안에 넣으면 유용할 수 있으며 -f 옵션을 사용하면 파일로부터 s/x/y/g와 같은 명령을 실행할 수 있습니다.
     ```C
     sed -f subst.sed inputFileName > outputFileName
     ```
   * 수정 편집 : GNU sed에 도입된 -i 옵션을 사용하면 파일의 직접 수정을 가능케 합니다.
     ```C
     sed -i 's/abc/def/' fileName
     ```
     
     
* **awk**
    * 정의: awk는 파일로부터 레코드(record)를 선택하고, 선택된 레코드에 포함된 값을 조작하거나 데이터화하는 것을 목적으로 사용하는 프로그램입니다. 즉, awk 명령의 입력으로 지정된 파일로부터 데이터를 분류한 다음, 분류된 텍스트 데이터를 바탕으로 패턴 매칭 여부를 검사하거나 데이터 조작 및 연산 등의 액션을 수행하고, 그 결과를 출력하는 기능을 수행합니다.
    * awk 명령어로 할 수 있는 일들
    
    ![99C761465D1CBF9B28](https://user-images.githubusercontent.com/94786955/142767027-c11eeed4-30c2-4168-a38e-a646822433c6.png)

    
getopt 출처: <https://soooprmx.com/c-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8-%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0%EB%A5%BC-%EC%B2%98%EB%A6%AC%ED%95%98%EB%8A%94-getopt-%EC%82%AC%EC%9A%A9%EB%B2%95/>,<https://en.wikipedia.org/wiki/Getopt>

getopts 출처: <https://en.wikipedia.org/wiki/Getopts>

sed 출처:<https://ko.wikipedia.org/wiki/Sed_(%EC%9C%A0%ED%8B%B8%EB%A6%AC%ED%8B%B0)>

awk 출처:<https://recipes4dev.tistory.com/171>
