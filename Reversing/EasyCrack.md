# Easy Crack(Reversing.kr)

![easy1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy1.PNG)



![easy2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy2.PNG)

프로그램을 실행했을 때 메시지 박스가 뜨며 입력창에 abcdefg 를 입력했을 때 실패창이 뜬다.



### 실행 1

![easy3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy3.PNG)

`sub     esp, 64h` esp 에 64 공간을 만들어 주었다.

`mov     ecx, 18h` ecx 에 18을 옮겼다.

`xor     eax, eax` xor eax 을 하면서 값을 0으로 만들어준다.

`lea     edi, [esp+68h+var_63]`  `[esp+68h+var_63]` 위치의 값을 edi에 넣어준다.

`mov     [esp+68h+String], 0`  `[esp+68h+String]` 위치 값을 0으로 만들어준다.

`push    64h`  d 가 들어간것을 들어간다.

`mov     edi, [esp+6Ch+hDlg]`  `esp+6Ch + 4` 를 `edi` 에 넣는다.

`lea     eax, [esp+6Ch+String]` 19F718 위치의 값이 eax 에 들어가면서 00이 되었다.

`call    ds:GetDlgItemTextA` eax 의 값이 7로 바뀌였다.

`cmp     [esp+68h+var_63], 61h ; 'a'` `esp+68h+var_63` 는 입력값의 2번째를 가리킨다.

2번째 값과 61 값을 비교한다. 여기서 2번째 값이 61 = a 인 것을 알아낼 수 있다.

---

### 실행 2

 ![easy4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy4.PNG)

`lea     ecx, [esp+6Ch+var_62]`  `esp+6Ch+var_62` 위치 값을 ecx에 넣는다. 2번째 값이 들어간 것을 알 수 있다. 

`push    offset a5y` a5y 가 들어가졌다.

`call    sub_401150` 부분에서 어떻게 실행 되는지 들어가본다.

---

#### call sub_401150

![easy5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy5.PNG)

`mov     ecx, [ebp+arg_8]` 2번째 값이 ecx 로 옮겨진다.

`mov     ebx, ecx` ecx 를 ebx 로 옮긴다.

`mov     edi, [ebp+arg_0]`  ebp+arg_0 위치의 값을 edi로 옮긴다.

` mov     esi, edi` edi 를 esi 로 옮긴다.

`xor     eax, eax` xor  비교를 하면서 eax 을 0으로 만들어준다.

`add     ecx, ebx` 2+0 = 2 이므로 ecx 값이 2로 바뀌였다.

`mov     esi, [ebp+arg_4]` 로 되면서 5y 값을 esi 로 옮겼다.

`mov     al, [esi-1]` esi 의 값을 한자리 씩 빼서 al에 옮긴다.

`xor     ecx, ecx` ecx 값이 0으로 된다.

`cmp     al, [edi-1]` edi에 있는 값들을 -1 해서 하나씩 al 과 비교한다.

esi 는 5y 가 들어가져있으며 그 값으로 al 과 비교하는 것이므로 aa5y를 적었을 때 값을 하나씩 빼면서 확인하였을 때 값이 같으므로 **call sub_401150** 에서 나와진다.

---

### 실행 3

![easy6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy6.PNG)

`mov     esi, offset aR3versing` esi 에 R3versing 이 들어가진 것을 알 수 있다.

`lea     eax, [esp+70h+var_60]` 에 입력 값이 들어간다.

![easy7](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy7.PNG)

62 가 [esp+70h+var_60] 인 19F71C 인 것을 알 수 있다.



`mov     dl, [eax]`  dl에 62 를 옮긴다.

`mov     bl, [esi]` bl 은 52가 된다.

`cmp     dl, bl` 를 비교를 한다.

52 가 같으면 밑으로 넘어가지만 같지 않을 경우 점프가 되면 실행 창으로 넘어간다.

결국 다음 글자는 R 인것을 확인 할 수 있다.

---

![easy8](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy8.PNG)

R 값을 넣어서 실패 창으로 점프되지 않고 밑으로 내려가진다. 그 이후 명령어들을 확인해본다.

`mov     dl, [eax+1]` 처음 비교했던 문자 위치 값에서  +1 을 하여 다음 값을 가져온다.

`mov     bl, [esi+1]` 암호화 된 문자 위치 값 또한 +1 하여 다음 값을 가져온다.

`cmp     dl, bl` 서로 값을 비교하여 같은지 아닌지 확인한다.

다음 값은 33 = 3 인 것을 알 수 있다. 이렇게 반복문을 돌면서 처음 위치 값에서 +1 을 하면서 값을 불러와 비교하는 것을 보고 암호를 알 수 있다.

---

![easy9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy9.PNG)

`cmp     [esp+68h+String], 45h` esp+68h+String 부분은 첫 앞글자이다. 

String= byte ptr -64h 이므로 첫글자의 위치를 표시한다는 것을 알 수 있다.

45는 E 값 이며 첫번째 값이 E 라는 것을 알 수 있다.

![easy10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/easy10.PNG)

이렇게 해서 Congratulation 메시지 창으로 가게 되었다.