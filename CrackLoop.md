## Crackme2 암호화 루프

Algorism 부분을 이어서 디버깅을 하다보면 사진과 같은 부분이 나온다.

![cr2ma](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/cr2ma.PNG)

`MOV EBX 4` 는 루프 반복을 몇번을 할 것인지 나타내는 명령어이다.

> **4** 인 이유는 Name 을 4글자 이상 쓰게 만든다. 그 이유가 아닐까 라는 생각을 해본다. 

---

![loa](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/loa.PNG)

**4031F0** 을 디버깅하면 EAX의 4ADE2C 주소 덤프창에 a를 받은 것을 볼 수 있다.

---

![lo61](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/lo61.PNG)

**4031F7** 를 실행하면 EAX 값이 ASCII(16진수) 값으로 변환된다.

---

**403233** 까지 디버깅하면 EBP-D4 에 64 값이 입력된게 보인다.

![lo64](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/lo64.PNG)

---

![lore](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/lore.PNG)

**4D3243** 까지 디버깅을 하면

EAX = 19F114 = 2

ECX = 19F154 = 0

EDX = 19F19C = 2

19F11C = d = 64

10F1A4 = a = 61

로 정리된다.

---

![lo1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/lo1.PNG)

**403243** 은 vbaVaradd() 함수를 실행시키면서 암호화 연산을 하였다.

![lo2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/lo2.PNG)

**40325B** 까지 실행하면 유니코드가 생성된것을 확인할 수 있다.

**RtcHexVarFromVar()** 함수는 다시 ASCII 코드를 유니코드로 변경해주는 함수이다.

이걸 반복을 4번하여 최종 암호화 값이 생성된다.

---

1. 입력한 Name을 이용하여 앞에서부터 한 문자씩 읽는다.

2. 받은 Name의 문자를 ASCII(16진수)로 변환한다.
3. 변환된 숫자에 64를 더한다.(64는 프로그램으로부터 입력된 값이다. 이걸 이용하여 입력받은 값을 합하여 암호화로 만든다.)
4. 연산된 값을 다시 문자로 변환한다.
5. 변환된 문자를 4번 반복하면서 연결시키면 암호가 만들어진다.