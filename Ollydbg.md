## Ollydbg

[![main-main.png](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/main-main.png)](https://postimg.cc/XrncFjmj)

**1. Code Window** : 각종 커멘트 및 레이블을 보여주며, 코드를 분석하여 loop, jump 위치 등의 정보 표시를 한다.

**2. Register Window** : CPU 레지스터 값을 실시간으로 표시/ 수정 가능하다.

**3. Dump Window** : 프로세서에서 원하는 메모리 주소 위치를 16진수와 ASCII / 유니코드 값으로 표시/ 수정 가능하다.

**4. Stack Window** : ESP 레지스터가 가리키는 프로세스 스택 메모리를 실시간으로 표시/ 수정 가능하다.



---

[![maincode.png](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/maincode.png)](https://postimg.cc/5X9LyPQj)

**004011A0 - Address** : 프로세스 가상 메모리 내의 주소

**E8 67150000 - Instruction** : IA32(또는 x86) CPU 명령어 (instruction - 지시, 설명)

**CALL - Disassembled code** :  OP code를 보기 쉽게 어셉블리로 변환한 코드

**0040270C - Comment** : 디버거에서 추가한 주석



---

## Ollydbg 기본 명령어

**Ctrl + F2** : Restart - 다시 처음부터 디버깅 시작

**F7** : Step Into - 하나의 OP코드 실행(CALL 명령어를 만나면 그 함수 코드 **내부로 들어간다.**)

**F8** : Step Over - 하나의 OP코드 실행(CALL 명령어를 만나면 내부로 들어가지 **않고** 함수 자체 **실행만** 한다.)

**Ctrl + F9** : Execute till Return - 함수 코드 내에서 RETN 명령어까지 실행한다.(함수 탈출 목적)

**Ctrl + G** : Go to - 해당 주소로 이동

**F4** : 현재 위치까지 실행

**F2** : Break Point(BP) 지정

**F9** : BP까지 실행

**Alt + B** : Breack Point 목록 실행

**;** : 주석(주석 입력창 실행)

**:** : 레이블(레이블 입력창 실행)

---

[![mainwin32.png](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/mainwin32.png)](https://postimg.cc/3Wc2NjCZ)

빨간색 글씨는 코드에서 호출되는 API 함수 이름이다.



---

## Hello World 코드

```c
#include "windows.h" 
#include "tchar.h" 
int _tmain(int argc, TCHAR *argv[]) 
{ 
    MessageBox(NULL, L"Hello World!",
               L"www.reversecore.com", 
               MB_OK); 
    return 0; 
}
```

**arg**(argumnet) : 함수의 독립변수

**argc** : arg + c(count) : 갯수. 프로그램을 실행할 때 지정해 준 **명령행 옵션의 갯수**를 뜻한다.

**argv** : arg + v(value) : 값. 프로그램을 실행할 때 지정해 준 명령행 옵션의 **문자열들 실제로 저장되는 배열**이다.

argc 의 값 3

​	프로그램 -copy "복사될 폴더" "복사할 폴더"

​			[0]	[1]				[2]					[3]

argv 의 값 4개. 4개의 값을 가지고 있는 문자열 배열이다.



![mainwin32](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/mainwin32.PNG)

**4010E4** : 주소에 들어갈 시 Win32 API 호출 코드로 내부로 들어가게 되면 반복문이 존재해서 함수 탈출까지 오랜시간이 걸린다.

**Win32 API** : **_stdcall** 함수 호출규약을 사용하기 때문에 가변인자를 맏지 않아 **Command Line**을 받아 오는 것이 필요하다.

**_stdcall** : Win32 API 함수 호출에 사용. 호출 수신자가 스택을 정리하므로 컴파일러는 **vararg** 함수를 **_cdecl**로 만든다. 이 호출 규칙을 사용하는 함수에는 함수 매개변수가 필요하다.

**vararg** : 가변인자 함수이다.

**_cdecl** : C 및 C++ 프로그램의 기본 호출 규칙이다. 스택은 호출자에 의해 정리되기 때문에 **vararg** 함수를 수행할 수 있다. **_cdecl** 호출 규칙은 각 함수 호출에 스택 정리 코드를 포함시키기 때문에 **_stdcall** 보다 큰 실행 가능 명령문을 생성한다.

어셈블러에서 **_stdcall** 은 내부에서 인자를 한번만의 호출로 정리 할 수 있는데
**_cdecl**은 인자를 지우기 위해서 몇번의 호출 과정이 더 있기 때문에 더 크다.
이러한 차이로 속도도 차이날 수 있다. 

**GetCommandLineW** : 현재 프로세스에 대한 명령줄 문자열을 검색한다.



---

[![main64bit.png](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/main64bit.png)](https://postimg.cc/k6P4Hc87)

**4010EE** : 32bit Ollydbg 사용하면서 64bit 환경에서 이용시 버그로 인하여 멈춤 현상이 생긴다.

주소 변환 루틴 문제로 인해 발생하는 것이다. 32bit 응용 프로그램 표준 2GB 메모리 주소 공간을 넘어 GetHostByAddress() 또는 GetHostByName()같은 네트워크 관련 API를 사용 하 여 할당 되는 사용자 모드 메모리 버퍼를 전달한다. 주소 다음 32 비트 주소를 커널 루틴에는 64 비트 주소를 변환한다. 

주소 변환 루틴 잘못 서명-확장 변환 64 비트 주소에 특정 주소. 이 64 비트 주소를 사용자 모드 주소 풀의 범위를 벗어난다. 따라서 커널 읽기 또는 쓰기 액세스 주소를 거부 및 응용 프로그램에 오류를 반환한다.

---

## Main() 함수 찾기

4010E4 와 4010EE 부분을 제외하고 CALL HelloWorld 가 들어간 부분을 들어가다 보면 MessageBox을 찾을 수 있었다.





> 참고 사이트
>
> [리버싱](https://blog.naver.com/hungjaksm/40200715272)
>
> [코드](https://euntitch.tistory.com/23 [eun._.titch])
>
> [arg](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=assortrockp&logNo=220671347945)
>
> [stdcall](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=work1989&logNo=221275066623)

> 작성날짜 010222