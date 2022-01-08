## Crackme2

프로그램 실행 후

[![me2.png](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/me2.png)](https://postimg.cc/qtXY5vTZ)

name 과 serial 을 넣었더니 wrong 메시지를 보여준다.

이 메세지를 보고 이벤트 시작점을 찾고 비교부분을 찾을 수 있을 것이다.

401238 - EP 부분이다.

push 401e14 를 하고 있다.

401e14에서 순서대로 VB5! 라고 char 을 표시하고 있다.

VB 전용 엔진을 사용하는 Visual Basic으로 제작되었다. 

> ![vb5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/vb5.PNG)
>
> VB5!를 확인할 수 있다.

---

##### 간접호출(Indirect Call)

> ![tecall](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/tecall.PNG)
>
> 간접 호출기법이다.
>
> 401238 → 40123D → 401232 (Indirect Call 방식이라고 부른다.)
>
> RT_MainStruct 구조체 주소를 스택에 입력하므로  RT_MainStruct 의 401E14가 주소이다.

---

**40123D - CALL JMP .&MSVBVM60.#100 ** : 401232 의 주소이다.

`MSVBVM60` 이란?

MSVBVM60.dll 은 사용자가 Visual Basic 프로그래밍 언어로 작성된 응용 프로그램을 실행할 수 있고 사용자 PC에 안전하도록 해주는 Window의 구성 요소 인 Visual Basic Virtual Machine 과 관련된 파일이다.

---

**401231 ThunRMain에 접속 후 보이는 화면**

![ThunRMain](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/image-20220103170618158.png)

MessageBox를찾기가 힘들기 때문에 간단한 방법으로 MessageBox를 찾아보겠다.

> RT_MainStruct 구조체이고 ThunRTMain의 인자로 프로그램 실행에 필요한 구조체 정보가 담겨져있다.

---



#### MessageBox 찾기

> 마우스 우클릭 → serch for → All referenced text strings



![textstrings](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/textstrings.PNG)

이 방법으로 프로그램을 실행하면서 본 메시지를 찾을 수 있다.

더블클릭을 하면 그 메시지가 있는 주소로 가게 된다.

![tewrong](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/tewrong.PNG)

메시지박스 호출 코드를 볼 수 있다. 이건 Wrong 에 해당되는 부분이다. 

---

##### Right MessageBox

![textright](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/textright.PNG)

위로 올라갔더니 Right 메시지 박스를 볼 수 있었다.

이렇게 보면 사용자가 **입력한 값**을 비교하여 **실패** 와 **성공**을 판단한다는 것을 알 수 있다.

---

##### 조건 분기

![teje](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/teje.PNG)

**403329** 까지 실행하고 Name 과 Serial 을 넣는다. 

`vbaVarTstEq 암호를 검사하는 함수`

**40332F** TEST 는 AX가 0 이면 ZF == 1 로 만든다. ZF가 1일 경우 점프를 하고 0일 경우 점프를 하지 않는다. 

하지만 Name - aaaa, Serial - aaaa 를 넣었을 때 점프를 하였다. 결국 AX는 0인 걸 예측 할 수 있다.

> `TEST opr1 opr2`
>
> 기능: 두 피연산자 사이에 논리적인 AND 연산을 수행하여 ZERO 플래그에 영향을 주지만 결과값을 **저장하지 않는다.**
>
> opr1 과 opr2 의 값이 0인지 아닌지 확인을 한다.

**403332** 에서 조건 분기 부분인 JE 명령어가 있다.  Name - aaaa, Serial - aaaa 를 넣었을 때 점프를 **403408** 하였다. 결국 AX는 0인 걸 예측 할 수 있다.

---

![teregi](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/teregi.PNG)

실행 후 레지스터 부분을 보면 EAX - 19F1BC 와 EDX - 19F1AC 인 것을 확인한다.

이 부분이 무엇을 담고 있는지 덤프창에서 확인 할 수 있다.

> 마우스 우클릭 → Long → Address with ASCII dump

![tename](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/tename.PNG)

Name 과 Serial 에 입력한 값들이 보인다.

그 중간에 보이는 UNICODE 가 Serial 값이다.

---

##### Success

![teyeah](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/teyeah.PNG)

실제 Serial 값을 입력시 나오게 된다.

다음은 Algorism 을 통해 시리얼 생성이 어떻게 되는지 알아보겠다.



> 참고 사이트
>
> [Crackme2](https://blog.naver.com/hungjaksm/40201288214)
>
> [간접호출](https://mm0ck3r.blog/46)



> 작성일
>
> 01/06/2022
