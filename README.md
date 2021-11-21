# OpenSW_assignment1
OpenSW_assignment1

이것은 **getopt, getopts, sed, awk** 명령어에 대한 설명입니다.


* 쉘 스크립트 관련 명령어
  * **getopt**
    * 정의:
    **getopt** 는 파이썬 기본 라이브러리 중 하나로 **명령행** 옵션을 처리해주는 모듈이다. **명령행**에서 입력된 옵션들을 정의하고 설정한 변수 값으로 할당해준다. 옵션 인수는 유닉스 getopt() 함수와 동일하게 – 혹은 -- 로 시작한다.
    * 원형: ``` int getopt(int argc, char * const argv[], const char *optstring); ```
    * 파라미터: argc, argv : main() 함수가 받은 파라미터를 그대로 전달한다. optstring : 파싱해야 할 파라미터를 쓴다. 옵션이 별도의 파라미터를 받는 경우 콜론을 함께 쓴다.
