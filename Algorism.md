## Crackme2 - Algorism

#### ❔Algorism

입력된 자료를 해결하려는 방법과 절차이다.

연산, 데이터 진행 또는 자동화된 추론을 수행한다.

---

### 이전 과정

![cralgo](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/cralgo.PNG)

![te](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/tename.PNG)

403329에서 실행했을 때 프로그램 창이 뜨고 Name 과 Serial 을 넣었더니 스택 부분에 실제 UNICODE"Serial" 이  넘어간 것을 확인 할 수 있었다.

Check을 누르면 Name 을 가지고 Serial을 만들고 JE를 통과하여 결과를 알려주는 순서이다.

---

## Serial 생성 과정

이제 Serial 이 생성되는 과정을 알아보겠다.

403329 에서 실행했을 때 프로그램 창이 뜬다는 것을 확인하였다. 그래서 함수의 시작 부분이  이보다 더 위 상단에 있다는 것을 예상할 수 있다. 

![alhand](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/alhand.PNG)

SE handler intallation 인 부분을 보고 handler 시작부분이라고 생각할 수 있다.

402ED6 - Check 버튼의 Event Handler 부분이다.

하지만 `PUSH EBP` ,`MOV EBP, ESP` 를 보고 **Stack Frame**의 시작부분인 것을 알 수 있어야한다.

스택 프레임이 끝이 날때까지 PUSH MOV 등 많은 명령어가 진행되고 있다.

하지만 이 프로그램에서 알 수 있는 것은 입력한 문자열을 읽어내는 함수가 있다는 것이다. 그러므로 CALL 위주로 **스택, 레지스터, 덤프창** 을 보기로 한다.

---

![alebp](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/alebp.PNG)

**402E00** 에서 디버깅을 하면서 처음 만난 CALL 부분이다.

**402F10** CALL 명령어 실행 후 EAX 값이 4가 되었다.

![alcall](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/alcall.PNG)

CALL 부분이 4번 돌면서 마지막 EBP-88 부분을 보면 UNICODE"aaaa" 들어간 것을 볼 수 있다.













> 참고 사이트
>
> [Crackme2](https://blog.naver.com/hungjaksm/40201426563)



> 작성일
>
> 01//2022