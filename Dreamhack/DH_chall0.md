# DH_chall0

![chall00_1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_1.PNG)

Dreamhack 의 chall0 파일을 열었을 때 보이는 첫 화면이다.

전체적 흐름을 보면 call 명령어로 무엇인가 불러내고 마지막에 비교를 한 후 Correct, Wrong 으로 메시지를 띄우는 것으로 예상해본다.

---

### call    sub_7FF604F91190

![chall0_3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_3.PNG)

`call    sub_7FF604F91190` 에 `call    cs:__acrt_iob_func` 을 들어가보았다.



![chall0_4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_4.PNG)



`__cdecl` 간접 호출이 보인다.

---

### call    sub_7FF604F91060

![chall0_9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_9.PNG)

![chall0_10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_10.PNG)

`call    sub_7FF604F91060` 에 `call    cs:__stdio_common_vfprintf` 을 들어가 보았다.

2번째 이미지처럼 보인다. 메인 부분의 printf 인 것을 알 수 있다.

---

### call    sub_7FF604F911F0

![chall0_6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_6.PNG)

![chall0_11](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_11.PNG)

`call    sub_7FF604F910B0` 까지 들어가보았다.



![chall0_7](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_7.PNG)

`call    cs:__stdio_common_vfscanf` 가 보인다.

![chall0_12](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_12.PNG)

scanf 은 입력 함수로 입력을 받게하므로 실행 창에 Input :  형식으로 입력 값을 넣게 만들어두었다.

여기에 abcd 입력 값을 넣었다.

---

![chall0_13](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_13.PNG)

RCX 는 `E1772FFCB0` 이며 그 안에 있는 것은 입력 값 abcd 가 들어가져있다.

---

### call    sub_7FF737541000

![chall0_14](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_14.PNG)

![chall0_15](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_15.PNG)

조건식이 보이므로 여기서 입력 값을 불러와서 값이 같은지 아닌지를 확인하는 부분 같다.

`lea     rdx, Str2` 명령어가 실행되면서 rdx 는 Compar3_the_str1ng 이 들어가졌다.

`mov     rcx, [rsp+38h+Str1]` 입력 값이 rcx로 옮겨졌다.

`call    strcmp` 문자열 비교 함수이다.

여기서 입력 값과 파일에 있는 값이 같은 경우 Correct 로 아닐 경우 Wrong 으로 간다는 것을 예측 할 수 있다.

`test    eax, eax` 부분에서 비교를 하고 Wrong 메시지로 넘어갔다.

입력 값이 문자열 비교 함수에서 다르다고 판단이 되어 Wrong 이 나왔을 것이다.

비교 함수가 나오기전에 `Compar3_the_str1ng` 가 rdx 에 들어가고 입력 값을 비교하는 것을 보면

정답은 Compar3_the_str1ng 이 들어가야한다.

![chall0_16](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall0_16.PNG)

Compar3_the_str1ng 입력 하면 Correct 메시지가 뜬다.

