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

EntryPoint는 401238 이고 **Push 401E14**를 가리키고 있다.

![Cra40](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra40.png)

401E14 를 검색해서 들어가보니 VB5! 가 적어져있다.

VB5! 가 무엇인지 검색 시 Visual Basic 의 약자이다.

**40123D** 가 CALL `JMP.&MSVBMVM60.#100` 을 한다. 그러면 **401232** 로 이동한다.

**401232** 에 ThunRTMain 이라고 적혀져있어서 Main 이라고 생각된다.

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

**403332** `JE 403408` 가 JE 조건 분기 하며 ZF가 1이면 점프하고 ZF 0이면 점프하지 않는다.

---

어디로 점프하는지 **403408** 을 검색해보았다.

![Cra3408](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra3408.png) 

Wrong 부분 근처로 옮겨지는 걸 확인할 수 있다.

처음 프로그램을 실행 시 wrong 부분으로 들어간 것을 생각할 수 있다.

---

![CraJ](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraJ.png)

**40332F  `TEST AX AX`** 무엇을 비교하는지 알기 위해 레지스터가 어떤 값을 가지고 있는지 확인해본다.

조건분기에서 `AX`으로 인해 메시지 박스가 달라지기 때문이다.

**403329  ` CALL vbaVarTstEq` **에서 함수, 스택의 값들을 `AX`의 리턴값으로 바꿔주는걸 예측할 수 있다.

---

![Cra66](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra66.PNG)

**403327 `PUSH EDX`**  레지스터 부분을 보면 19F1AC 가 들어간 것을 확인 후 스택 창을 보면 이미 값이 들어간 것을 볼 수 있다.

**403328  `PUSH EAX`** EDX와 같이 값이 8 들어간 것을 확인 할 수 있다.

레지스터에서 보여주는 주소를 검색 시

이 값 중에 하나는 프로그램 실행 시 넣었던 입력 값인 것을 확인할 수 있었다. 그리고 입력값과 입력값으로 생성된 값을 비교하는 부분이 있었고 그것으로 비교하는 다른 하나의 값이 Serial인 것을 알 수가 있었다.

---

![CraE](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraE.PNG)

![CraYe](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraYe.PNG)



---

## Serial 생성❔

시리얼 키를 **찾는** 과정을 하였다면 생성 과정을 찾아보겠다.

시리얼 **찾는** 과정에서는 메시지 박스를 이용하여 위치를 찾고 조건분기를 찾아서 알게 되었다.

이번에는 조건 분기가 포함되어 있는  Wrong 과 Yep 을 띄우는 함수를 찾아본다.

---

![Cra1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra1.PNG)

한참 위를 올리다 보면 SE handler installation 이라는 것을 볼 수 있다.

Check 버튼 함수 부분이라고 생각할 수 있겠다.

`Push EBP` , `Mov EBP, ESP` 를 보면 스택프레임 부분이 보인다.

함수 부분의 시작인 것을 알 수 있다.

**402ED6** : SE handler installation. 입력 값을 넣은 것들이 스택, 덤프창에 검색 시 나오지 않는다.

---

![1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/1.PNG)

**402EE9 `SUB ESP, 144`**  144까지의 공간을 만들어준다. 

`PUSH EBX , PUSH ESI, PUSH EDI` 를 스택에 넣는다.

**402EF2 `MOV [EBP-C], ESP`** ESP 값이 [EBP-C] 에 옮겨진다.

**402EF5 `MOV [EBP-8], abexcm2-.004010F0`** [EBP-8] 위치에 들어가져있다.

 ![2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/2.PNG)

**402EFC `MOV ESI, [EBP+8]`** 

![3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/3.PNG)

16진수 값이 들어가져 있다.

---

![Cra2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra2.PNG)

**402EFF `MOV EAX,ESI`** 16진수의 "Be" 의 값이 EAX로 옮겨졌다.

**402F01 `AND EAX,1` **

![4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/4.PNG)

EAX 값이 1이 되었다.

**402F04 `MOV [EBP-4], EAX`**  [EBP-4] 로 EAX가 옮겨졌다.

**402F07 `AND ESI, FFFFFFFE`** ESI = ESI & FFFFFFFE 가 되는데 주소 검색 시 나오는 것이다.

![5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/5.PNG)

**402F0B `MOV [EBP+8], ESI`** ESI가 [EBP+8]에 옮겨지고

**402F0E `MOV ECX, ESI`** ESI 가 ECX로 옮겨졌다.

**402F10 `CALL [ECX+4]`** (Zombie_AddRef)

![6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/6.PNG)

CALL 을 하면서 레지스터들의 값들이 바뀌였다. EAX는 4가 되었고

스택창 덤프창 레지스터를 검색해서 찾아보아도 MSVBVM 관련 주소 부분들이 보였다.

**402F15 `XOR EDI, EDI`** 0을 넣어주고 **402F18 ~ 402F72** 까지 **402EE9 `SUB ESP, 144`** 에서 만든 공간들에 EDI 를 하나씩 옮겼다.

![Cra3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra3.PNG)

**402F78 `CALL [EDX+308]`** 검색해서 찾아보아도 0000 이고 입력값이 찍힌 것을 확인하지 못하였다.

**402F7F `LEA EAX, [EBP - 8C]`**  [EBP - 8C] 주소 값을 EAX에 넣었다. 아무것도 적혀있지 않다.

**402F86 `CALL vbaObjSet`** 검색 시 MSVBVM60 관련된 것들이 나온다.

---

![7](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/7.PNG)

**402F8E `LEA EDX, [EBP-88]`**  EBP -88 주소 값을 EDX에 넣는다. 아직까지 값이 들어간 것은 없다. 하지만 name 저장공간 만들어준다.

**402F94 `PUSH EDX`** 402F8E LEA EDX, [EBP-88] 에서 받은 주소 값을 넣는다.

**402F95 `PUSH ESI`**  MSVBVM60 관련 된 것을 넣는다.

**402F96 `MOV ECX, ESI`** ESI를 ECX로 옮긴다. 

**402F8E ~ 402F96**  name을 받아오기 위한 과정이다.

name을 받아 오기 위한 공간을 만들어주고

**402F98 `CALL [ECX+A0]` **를 불렀을 때 EBP-88 을 확인 하면 name 의 입력 값을 볼 수 있다. name의 입력 값을 불러왔다.

![8](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/8.PNG)

![9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/9.PNG)

`CALL` 이 되면서 레지스터의 값이 변경되었지만 `[EBP-88]` 을 보면 입력값이 잘 들어간 것을 확인 할 수 있다.

---

이후 디버깅을 계속 실행 시 **4030F9** 로 넘어오게 된다.

넘어오게 된 부분을 보면 **4030F9** 전 까지의 내용은 4글자 이상 입력하라는 메시지 박스가 있다.

![10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/10.PNG)

4글자 이상 입력하는 문구인데 4글자 이상을 입력하였기에 조건분기 부분에서 **4030F9** 로 넘어오게 되었다.

---

디버깅을 더 실행 했을 때 **403102 `MOV EBX, 4`** 에 EBX 가 4로 변하였고 스택에 EDX, EAX 를 넣었다.

![11](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/11.PNG)

**403109 `MOV [EBP-4], EBX`**

![12](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/12.PNG)

값이 들어간걸 확인할 수 있으며 **40310F `MOV [EBP-DC], 8002`** 도 값이 들어간다.

**403119 `CALL vbaLenVar`** vbaLenVar(arg1, arg2) arg2의 스트링 길이를 구해 arg1에 저장을 하는 함수이다.

레지스터는 MSVBVM60 관련된 것을 가지게 될 것을 예상한다.

**403127 `CALL vbaVarTsLt`** 시스템에서 정해진 값보다 작을 시 ffffff를 리턴하는 명령어이다.

**40312D `TEST AX, AX`** 입력값을 비교하여 글자 갯수를 확인하고 참이므로 **403130  `JMZ 4034F3`** 에서 4034F3으로 점프하지 않는다. **4034F3** 는 이벤트가 끝나는 부분이다.

**403136 ~ 403185** 레지스터들의 값들이 옮겨지고 넣어지는 과정이다.

**403191 `CALL vbaVarForInit`** 

![13](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/13.PNG)

레지스터의 값이 바뀌었는데 반복되면서 찍히는 횟수 값이라고 예상한다.

---



![HO](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra9.PNG)

**403191 `EBX rtcMidCharVar`** name에 대한 n번째 문자열을 저장한다.

불러온 n번째 값을 저장했다고 예상한다.

**403197 `TEST EAX, EAX`** 입력값과 시스템값을 비교한다.

**403199 `JE 4032A5`** TEST EAX 비교 값을 확인 후 4032A5로 갈지 안갈지가 된다.

4032A5는 반복문이 끝나는 부분이다.

---

**4031BA `CALL vbaI4Var`** name의 한 글자를 저장한다. EAX에 저장된다고 한다. 그래서 1인 거라고 생각해본다.

![14](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/14.PNG)

**4031C0 ~ 4031DF** 레지스터에 값이 넣어지거나 옮겨졌지만 검색 시 입력값이 보이지 않는다.

![15](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/15.PNG)

![16](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/16.PNG)

---

**4031F0 `CALL vbaStrVarVal`** EAX 가 변경되면서 검색 시 입력 값이 적혀져있는 것을 보며 문자의 포인터를 리턴해주는 것을 예상할 수 있다.

 ![17](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/17.PNG)

**4031F7 `CALL rtcAnsiValueBstr`** EAX 값이 61 값으로 변경되었다.

16진수 값으로 변경해주는 것이라고 예상해본다.

![18](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/18.PNG)

---

![19](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/19.PNG)

**4031FD `LEA EDX, [EBP-DC]`** 2값을 EDX에 넣는다.

**403203 `LEA ECX, [EBP-54]`** 2F0008 을 ECX에 넣는다.

---

**403206 `MOV [EBP-D4], AX`** AX의 값이 [EBP-D4] 로 옮겨졌는데 61 = a 의 값이다.

![31](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/31.PNG)

**40323D `MOV [EBP-D4], 64`** 19F11C 이전에 61 = a 가 있는 걸 보았는데 `MOV [EBP-D4], 64` 이 넣어진 것을 볼 수 있다. 

![32](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/32.PNG)



**4403243 `CALL vbaVarAdd`** 

![26](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/26.PNG)

`EDX` 부분에 `vbaVarAdd` 함수 발생되면서 입력값과 시스템에 저장되어 있는 값을 더해준 값을 확인할 수 있다.

![33](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/33.PNG)

---

**403249 ~ 40325A** 이전 값들이 옮겨지는것을 확인할 수 있다.

**40325B `CALL rtcHexVarFronVar`** 

![27](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/27.PNG)

![28](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/28.PNG)

`rtcHexVarFronVar` 함수를 사용함으로써 `vbaVarAdd` 에서 더한 값들을 다시 16진수로 바꾼 것을 예상할 수 있다.

---

![29](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/29.PNG)

**4032A0 `JMP 403197`** 정해진 루프가 다 반복될때까지 403197로 점프한다. 403197은 루프의 시작부분이다.

`JMP 403197` 명령어가 실행 되기 전까지 입력값과 암호화값이 변경되는 부분이 없어보인다.

반복문 처음으로 돌아가서 **4031F0 `CALL vbaStrVarVal`**  에서 다시 값이 새로 찍히며(name을 abcd 작성 시)

![36](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/36.PNG)

**40325B `CALL rtcHexVarFronVar`** 

![34](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/34.PNG)

name의 2번째 문자를 받아서 암호화 한 것을 볼 수 있다.

**40327B CALL vbaVarCat** 이 부분을 지나면서 암호화가 하나씩 생성된 것이 이어지는 것을 확인할 수 있다.

![35](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/35.PNG)
