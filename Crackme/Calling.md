## 함수 호출(3)

- cdecl
- stdcall
- fastcall



#### cdecl 방식

cdecl 호출하는 쪽이 스택을 정리하기에 호출할때마다 스택을 정리하는 부분이 있지만 가변 길이 파라미터를 전달 할 수 있다.

함수 파라미터가 스택에 들어간다. 스택 종료 시 반환되야 한다.

함수 호출 전과 후 같아야 한다. 그래서 들어간 인수 크기 만큼  ADD ESP 해주며 공간을 없애준다.

---

```c
#include "stdio.h"
int add(int a, int b)
{
    return (a + b);
}
int main(int argc, char* argv[])
{
    return add(1,2);
}
```

![calling1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/calling1.PNG)

---

401010 `PUSH EBP` 이전 스택 값을 넣는다.

401011 `MOV EBP, ESP` ESP를 EBP로 옮긴다.

401013 `PUSH 2`

401015 `PUSH 1`

먼저 2를 넣은 이유가 리틀엔디언 기법을 사용하기 때문에 C언어 메인 코드에서 먼저 1이 오기 위해 `PUSH 2` 를 먼저 해주었다. 파라미터를 역순으로 넣어서 2를 먼저 스택에 넣어지게 하였다.

401017 `CALL 401000` 401000 으로 가서 계산을 하고

401009 `POP EBP` 로 이전 스택으로 돌아간다.

40101C `ADD ESP, 8` 을 해주고 공간을 없앤다.

40101F `POP EBP` 이전 스택으로 돌아간 후 끝난다.

---

#### stdcall 방식

함수가 호출된 쪽이 스택을 정리한다.

함수가 리턴 후 스택 정리한다. 스택을 정리하는 것은 곧 한 곳이다.

```c
#include "stdio.h"
int _stdcall add(int a, int b)
{
    return (a + b);
}
int main(int argc, char* argv[])
{
    return add(1,2);
}
```

![calling2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/calling2.PNG)

---

401006 `ADD EAX,[EBP + C]` 까지 cdecl 함수 호출이랑 같다.

메인 부분에서 파라미터를 입력 받고 함수 부분에서 계산을 하였다.

401009 `POP EBP` 메인으로 돌아가기전에 이전 스택으로 값으로 돌아간다.

40100A `RETN 8` 스택을 정리한다.



40401C `POP EBP` 스택이 생기기전으로 값이 돌아간다.

40101D `RETN` 함수가 끝난다.

---

#### fastcall

함수가 호출된 쪽이 스택을 정리한다.

빠른 함수 호출이다. ECX, EDX 레지스터(2개)에 넣어지는 것을 볼 수 있다. 하지만 ECX, EDX 레지스터를 관리하는 추가적인 오버헤드 필요하다.

스택에 쌓이지 않고 레지스터에 넣기 때문에 속도가 빠르다.





> 참고 사이트
>
> [Calling convention](https://blog.naver.com/hungjaksm/40201523286)