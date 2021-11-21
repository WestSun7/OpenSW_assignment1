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
