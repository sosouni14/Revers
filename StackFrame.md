# Stack Frame

#### ❔스택 프레임이란?

ESP가 아닌 EBP를 이용하여 스택 내의 로컬변수, 파라미터, 복귀주소에 접근하는 기법이다.

ESP는 값을 넣고 뺄 때마다 변경되지만 EBP는 변경되지 않는다. 이후 A 라는 값이 해제되면 스택이 깨지지 않도록 EBP가 ESP에게 값을 돌려주어 ESP = EBP가 된다.



## 실습 코드 C++

```c++
#include "stdio.h"
long add(long a, long b)
{
    long x = a, y = b;
    return ( x + y );
}
int main(int argc, char* argv[])
{
    long a = 1, b = 2;
    printf("%d\n", add(a,b));
    return 0;
}
```

---

###### 파일 실행 메인 화면

[![stackmain.png](https://i.postimg.cc/fLqFSb1L/stackmain.png)](https://postimg.cc/r0WgvqVX)

---

###### 코드의 흐름 순서 1

[![stack1.png](https://i.postimg.cc/QMqNPvyD/stack1.png)](https://postimg.cc/p9pt5cvc)



###### 코드의 흐름 순서 2

[![stack2.png](https://i.postimg.cc/CL7vR5DV/stack2.png)](https://postimg.cc/hfJ8wSpC)



###### 코드의 흐름 순서 3

[![stack3.png](https://i.postimg.cc/gkgDY5Fz/stack3.png)](https://postimg.cc/8jrW4Zz9)

C++의 코드 흐름이 이 순서대로 진행된다.

---



#### 코드의 흐름 순서 1

[![stack1.png](https://i.postimg.cc/QMqNPvyD/stack1.png)](https://postimg.cc/p9pt5cvc)



401020 - `Push EBP` :  `main()` 시작이다. EBP 가 메인함수에서 베이스 포인터의 역할이다. 그러므로 이전 값을 스택에 넣어서 스택이 깨져도 되찾을 수 있게 만든다.

⁙ EBP 값을 스택에 넣는다.

401021 - `MOV EBP, ESP` : 현재의 ESP를 EBP에게 같은 값을 갖게한다.

⁙ ESP 값을 EBP에게 복사한다.

401023 - `SUB ESP, 8` : long 은 4byte 이다. a, b 가 있으므로 총 8byte 가 필요하다.

⁙ 8byte 공간 확보한다.

401026 - `MOV DWORD PTS SS: [EBP-4],1 ` : a의 1을 [EBP-4] 에 저장한다.

⁙ [EBP-4] = 1

40102D - `MOV DWORD PTR SS:[EBP-8],2` : b의 2를 [EBP-8]에 저장한다.

⁙ [EBP-8] = 2

401034 - `MOV EAX,DWORD PTR SS:[EBP-8]` : 4byte 크기의 [EBP-8]주소 내용을 EAX로 이동한다.

401037 -  `PUSH EAX` : 2인 b의 값을 스택에 넣는다.

⁙ ESP 는 EAX = 2 인 것을 가르키고 있다.

401038 - `MOV ECX,DWORD PTR SS:[EBP-4]` : 4byte 크기의 [EBP-4]주소 내용을 ECX로 이동한다.

40103B - `PUSH ECX` : 1인 a의 값을 스택에 넣는다.

⁙ ESP 는 ECX = 1 인 것을 가르키고 있다.

40103C - `CALL 00401067` : add() 함수 이다.



---

#### 코드 흐름 순서 2

[![stack2.png](https://i.postimg.cc/CL7vR5DV/stack2.png)](https://postimg.cc/hfJ8wSpC)

401000 - `PUSH EBP` : main() 함수에서 사용된 EBP 값을 스택에 넣는다.

⁙ add() 함수의 시작이다.

401001 - `MOV EBP,ESP` : 현재의 ESP를 EBP에게 같은 값을 갖게한다.

⁙ ESP = EBP

401003 - `SUB ESP,8` : a, b 의 공간인 스택에 8byte 를 확보한다.

401006 - `MOV EAX,DWORD PTR SS:[EBP+8]` : 파라미터 a 를 가리킨다.

401009 - `MOV DWORD PTR SS:[EBP-8],EAX` : a의 값인 1을 [EBP-8]에 넣는다.

40100C - `MOV ECX,DWORD PTR SS:[EBP+C]` : 파라미터 b 를 가리킨다.

40100F - `MOV DWORD PTR SS:[EBP-4],ECX` : b의 값인 1을 [EBP-4]에 넣는다.

401012 - `MOV EAX,DWORD PTR SS:[EBP-8]` : 1을 EAX에 넣는다.

401015 - `ADD EAX,DWORD PTR SS:[EBP-4]` : EAX에 2를 더해서 EAX는 3이다.

401018 - `MOV ESP,EBP` : ESP의 값을 EBP에 넣어두었다가 함수가 종료될 때 ESP를 원래대로 복원시키다.

⁙ ESP를 원래대로 복원한다. 

---

#### 코드 흐름 순서 3

[![stack3.png](https://i.postimg.cc/gkgDY5Fz/stack3.png)](https://postimg.cc/8jrW4Zz9)



401041 - `ADD ESP,8` : ESP에 8byte를 더한다.

⁙ add() 파라미터를 제거함으로써 스택 정리가 된다.

401044 - `PUSH EAX` :  add() 함수의 리턴값

⁙ ESP 에 EAX = 3 인 것을 가르키고 있다.

401045 - `PUSH 40B384 ` :  "%d\n"

⁙ ESP 에 "%d\n" 인 것을 가르키고 있다.

40104A - `CALL 00401067` : printf()

40104F - `ADD ESP,8` : printf() 에 있는 파라미터 2개를 정리한다.

⁙ 스택 공간 정리한다.

401052 - `XOR EAX,EAX` : main() 에 return 0;  리턴값을 받은 EAX 초기화를 한다.

⁙ EAX 초기화, 0으로 만든다.

401054 - `MOV ESP,EBP` : 함수 시작했을 때의 값으로 복원한다.

⁙ ESP 정리한다.

401056 - `POP EBP` : return 전에 저장했던 원래 EBP 복원한다.

401057 - `RETN` : main() 함수 종료한다.