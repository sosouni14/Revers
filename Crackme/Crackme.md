## Crackme

Crackme 1 프로그램을 실행 시 메시지 가 보인다.

![CraMes](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraMes.png)

![CraME](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraME.png)

 HD 에 CD-Rom 을 만들라는 메시지가 나오며 확인을 누르면 CD-Rom 이 아니라는 메시지 창이 뜬다.

----

![CraMa](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraMa.png)

파일 실행시 보이는 메인 화면이다.

프로그램 실행 시 보였던 메시지창이 한 눈에 보이는 것을 확인 할 수 있다.

---

EntryPoint 부분 **401000** 이다. 디버깅을 하면 **40100E** 에서 MessageBoxA에 걸리면서 처음 나온 메시지 가 띄어진다.

![CraJE](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraJE.png)

**401026** 까지 실행 후 **401028** 로 갔다. 

**401026** 은 **JE 40103D**  조건 분기를 보여주고 있다. 고로 어떠한 값이 될 시 **40103D** 로 이동하고 아닐 시 **401028** 로 가는 것을 실행하면서 볼 수 있었다.

---

![CraD](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraD.png)

**401026** 이 조건 분기이므로 이전에 무언가 실행 되었으므로 조건 분기 부분에서 **401021** 로 점프를 하지 못하고 **401028**로 간 것을 알 수 있을 것이다.

> `INC` 는 oper 에 값을 1 증가 한다.

> `DEC` 는 oper 에 값을 1 감소 한다.

그러면 값을 증가 하고 감소하기 전의 레지스터 값을 확인해본다.

---

![Cra3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra3.png)

**40101D** 가 실행되기 전의 **EAX 값은 3**이다. **ESI 는 0**이다.

EAX 값이 3인 이유는 **GetDriveType** 의 반환 값이 드라이브 고정이라는 반환 값이 3이라서 그렇다.

**40101D** : INC ESI = 0 + 1 = 1 = ESI

![Cra1E](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra1E.png)

**40101E** : DEC EAX = 3 - 1 = 2 = EAX

**40101F** : JMP 401021 로 이동한다.

![Cra21](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra21.png)

**401021** : INC ESI = 1 + 1 = 2 = ESI

**401022** : INC ESI = 2 + 1 = 3 = ESI

![Cra23](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra23.png)

**401023** : DEC EAX = 2 - 1 = 1 = EAX

**401024** : CMP EAX, ESI = EAX 와 ESI 를 비교한다.

만약 EAX 와 ESI 가 같을 경우 JF 가 1 이고 같지 않을 경우 JF 는 0이 된다.

**401026** 디버깅 결과 **JE** 조건분기에서 **JF가 0** 이므로 40103D로 가지 못한 것이다.

---

## JE 40103D

조건 분기 **401024** 에서 **40103D** 로 실행 될 수 있는 방법이 총 3가지이다.

1. 명령어를 지운다.

2. 비교값을 없앤다.
3. EAX 값과 ESI 값을 CMP 실행 전에 같게 만들어준다.

---

##### 명령어 지우기

> 수정하고 싶은 주소에 space 를 눌러서 명령어를 수정

![CraS](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraS.png)

CMP 실행 전 ESI 값과 EAX 값이 같은 것을 볼 수 있다.

왜냐하면 401021 까지는 EAX 와 ESI 값이 같았다. 

하지만 수정 이전에는 값을 더 더한거나 빼게 되어서 값이 달라졌다.

값이 같아진 이후로는 명령어들을 NOP(아무것도 하지 않게 만드는 명령어) 으로 수정하여 CMP 명령어를 실행 했을 때 값이 같으므로 **401026** 의 **JE** 조건분기에서 JF 가 1이 되어 **40103D** 로 가서 실행되었다.

---

##### 비교값 없애기

![CraJm](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraJm.png)

**401024** 부분은 원래 CMP EAX, ESI 이다. 이것을 JMP 로 수정하였더니

EAX 값과 ESI 값이 다른 것을 비교하지 못하고 바로 40103D 로 점프한 것을 볼 수 있다.

---

##### EAX 값과 ESI 값을 CMP 실행 전에 같게 만들어주기

![Cra5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra5.png)

처음 EAX = 3 시작했고 ESI = 0 으로 시작하였다. 

그리고 CMP 실행 전에는 EAX = 1 이고 ESI = 3 으로 끝으로 비교문을 하게 되었다.

그러므로 EAX 는 DEC 명령어를 만나 - 만 되었으므로 EAX를 5로 고쳐주고

**401024** CMP 에서 두 값이 같아져서 **401026** JE 조건 분기에서 40103D 로 넘어가게 해주었다.

![Cra33](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/Cra33.png)

CMP 실행 전에 EAX 값과 ESI 값이 3인 것을 볼 수 있다.

**401026**  조건 분기에서 40103D로 실행된다.

---

![CraYeP](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/CraYeP.png)

3가지 방법으로 YEAH 메시지 박스를 띄울 수 있다.

