## Crackme2

Crackme 실행 시 Name 입력과 Serial 을 입력하는 창이 뜬다.

![CraP](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraP.png)

Name 과 Serial 을 입력하였다.

![CraN](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraN.png)

Name 에 3글자만 입력시 최소 4글자 적으라는 메시지창이 떴다.

![CraW](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraW.png)

Name 4글자 적고 Serial을 임의로 쓰니 틀렸다는 메시지창이 떴다.



파일을 실행 시 무엇이 나오고 어떠한 메시지 창이 뜨는 확인을 해보았다.

이것을 디버거 프로그램으로 확인해본다.

![Crackmain](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Crackmain.png)

EntryPoint는 401238 이고 **Push 401E14**를 가르키고 있다.

![Cra40](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra40.png)

401E14 를 검색해서 들어가보니 VB5! 가 적어져있다.

VB5! 가 무엇인지 검색 시 Visual Basic 의 약자이다.

**40123D** 가 CALL `JMP.&MSVBMVM60.#100` 을 한다. 그러면 **401232** 로 이동한다.

**401232** 에 ThunRTMain 이라고 적혀져있어서 Main 이라고 생각된다.

> Why?
>
> **401238** 에서 Push 401E14 를 하고 **40123D** 에서  CALL JMP.&MSVBMVM60.#100(401232 주소) 를 할까?
>
> Push 401E14 이후 401232 로 가기 어려운 걸까?
>
> 찾아도 이유는 그저 VB 에서 MSVBVM 를 사용하기 위한 간접호출 기법이다.

접속 후 메시지박스를 찾기 어렵기에 간단하게 찾는 방법으로 하겠다.

---

### Message Box

> 마우스 우클릭 → serch for → all regerenced text strings



![CraM](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraM.png)

프로그램 실행 후 입력 값을 넣었을 때 보았던 메시지 들을 볼 수 가 있다.

이미 입력 값에 올바르지 않은 것을 넣으면 Wrong serial 이 나오는 것을 알기에 어떻게 하면 Yep 문구를 나오게 할지 Wrong 부분을 더블클릭하여 그 주소부분을 확인해본다.

---

![CraWr](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraWr.png)

Wrong 부분에 해당되는 메시지 박스이다.

---

성공한 메시지 문구가 있는 것을 메시지 박스 검색 시 보았기 때문에 위로 올라가보았다.

![CraY](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraY.png)

Yep 부분에 해당되는 메시지 박스를 확인 할 수 있었다.

그러면 입력한 Serial을 판단하는 부분이 있다는 것을 예측할 수 있다.

---

![CraJ](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraJ.png)

아주 조금만 올리면 가까운 곳에 JE 로 나와있는 조건분기를 볼 수 있다.

**403332** 가 JE 조건 분기 하며 ZF가 1이면 점프하고 ZF 0이면 점프하지 않는다.

---

어디로 점프하는지 **403408** 를 검색해보았다.

![Cra3408](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra3408.png) 

Wrong 부분 근처로 옮겨지는 걸 확인할 수 있다.

처음 프로그램을 실행 시 wrong 부분으로 들어간 것을 생각할 수 있다.

---

![CraJ](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraJ.png)

**403332** 부분에서 무언가 ZF 를 1 로 만들었기에 **403408** 로 점프하게 만든 것이다.

**40332F** 부분을 보면 TEST 명령어가 있다. TEST 는 oper1 와 oper2 값 들이 0인지 확인한다.

TEST ax, ax 이므로 ax 가 0 일 경우 ax = ax 로 되어 결국 0이다. 그래서 ZF 가 1로 된 것을 알 수 있다.

그러면 **403329** 가 무엇을 CALL 을 하여 40332F 에서 TEST 를 하는지 **403329** 에 BP를 걸어서 확인해본다.

---

![CraE](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraE.png)

EP를 걸고 실행 시 프로그램 창이 뜨고 값을 넣은 후 레지스터를 보았다.

EAX 가 무엇을 담고 있는 지 확인을 해보니 그 주소값 근처 부분에 Name 값과 Serial 값이 있는 것을 확인 할 수 있었다.

입력 값 외에 다른 값인 C5C5C5C5 가 있다. 어떤 것인지 모르겠지만 Serial 값이 넣었더니 Yep 문구가 나온다.

![CraYe](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraYe.png)

어떻게 생성 되었는지 모르지만 이게 Serial 값인 것을 알 수 있게 되었다.



---

## Serial 생성❔

시리얼 키를 **찾는** 과정을 하였다면 생성 과정을 찾아보겠다.

시리얼 **찾는** 과정에서는 메시지 박스를 이용하여 위치를 찾고 조건분기를 찾아서 알게 되었다.

이번에는 조건 분기가 포함되어 있는  Wrong 과 Yep 을 띄우는 함수를 찾아본다.

---

![Cra1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra1.png)

한참 위를 올리다 보면 SE handler installation 이라는 것을 볼 수 있다.

Check 버튼 함수 부분이라고 생각할 수 있겠다.

`Push EBP` , `Mov EBP, ESP` 를 보면 스택프레임 부분이 보인다.

함수 부분의 시작인 것을 알 수 있다.

**402ED6** : Push handler. 입력 값을 넣은 것들이 스택, 덤프창에 검색 시 나오지 않는다.

![Cra2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra2.png)

**402F10** 부분에서 CALL 하면서 ECX+4 가 되었고 EAX 는 4가 되었다.

하지만 스택창 덤프창 레지스터를 검색해서 찾아보아도 MSVBVM 관련 주소 부분들이 보였다.

![Cra3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra333.png)

**402F78** 에서 CALL EDX+308 이다.

검색해서 찾아보아도 0000 이고 입력값이 찍힌 것을 확인하지 못하였다.

**402F7F** 에 EBP - 8C 의 주소 값을 EAX에 넣었다.

**402F86** CALL vbaObjSet 함수를 부른다.

**402F8E** EBP -88 주소 값을 EDX에 넣는다.

**402F94** Push EDX 받은 주소 값을 PUsh 한다.

**402F95** PUSH ESI 주소 저장한다.

**402F96** MOV ECX, ESI 를 복사한다.

**402F98** CALL ECX+A0 를 부른다.

아직 더 실행해야 된다고 생각된다. check 함수 안에서 어떻게 돌아가는지 잘 모르겠다.