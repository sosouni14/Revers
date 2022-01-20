# Easy Keygen(Reversing.kr)

![key1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key1.PNG)

파일 압축을 풀면 프로그램과 함께 텍스트 파일이 있다.

![key2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key2.PNG)

시리얼 넘버가 5B134977135E7D13 일 때 이름을 알아내라고 써져있다.

---

![key5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key5.PNG)

`[esp+140h+var_130], 10h`

`[esp+140h+var_12F], 20h`

`[esp+140h+var_12E], 30h` 

각각 10, 20, 30 값을 넣어주었다.

![key4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key4.PNG)

Input Name 이 생성 된 후 이름 값을 넣는 부분부터  실행하였을 때

`movsx   ecx, [esp+esi+13Ch+var_130]` ecx 가 10이 되었고

`movsx   edx, [esp+ebp+13Ch+var_12C]` edx 는 61 이다.

`xor     ecx, edx` 10 ^ 61 = 71 이다. ecx = 71 이 된다.

`inc     ebp` 0이 였던 값이 1 증가 되므로 입력된 값 0에서 1 위치로 간다는 것을 예측할 수 있다.

`lea     edi, [esp+13Ch+var_12C]` Input Name 으로 들어간 입력 값이 들어가져있다.

![key6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key6.PNG)

![key7](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key7.PNG)

입력 값을 abcdef 넣은 것을 볼 수 있다.

`or      ecx, 0FFFFFFFFh` or 을 하여 ecx 값을 FFFFFFFF 값으로 만들었다.

`xor     eax, eax` eax or 을 하여 0으로 만들어버린다.

`inc     esi` 0이였던 값을 증가하여 1이 되었다.

`repne scasb` ecx 이 FFFFFFF8 로 바뀌였다.

`not     ecx` ecx 가 00000007 로 바뀌였다.

`dec     ecx`  -1 를 하므로 00000006 으로 바뀐다.

`cmp     ebp, ecx`  1 과 6을 비교한다.

![key8](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key8.PNG)

반복된 횟수를 비교하고 같지 않으면 다시 반복하도록 만드는것을 예측할 수 있다.

---

![key9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key9.PNG)

이번에는 20 XOR 62 이다. 62는 입력 값의 1번째 이다.

결국 값은 42가 나온다.

---

![key10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key10.PNG)

2번째 반복 되었을 때 30 ^ 63 인 것을 알 수 있다. 63은 2번째 입력 값이다.

---

![key11](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key11.PNG)

3번째 반복되었을 때 3번 째 값과 XOR 10 이 된다.

이것을 보고 10, 20, 30 이 계속 반복 된다는 것을 예측 할 수있다.

---

![key12](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key12.PNG)

`cmp     ebp, ecx` 값을 계속 비교한다. ebp 의 값은 계속 증가 되며 ecx 값과 같은지 비교한다. 이것을 보고 반복 횟수를 체크하는 것이라고 알 수 있다.

---

![key13](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key13.PNG)

이렇게 입력 값을 넣어서 반복되면서 나온 값들을 Serial 에 입력 하였을 때 Correct 가 나오는 것을 볼 수 있다.

그러면 Serial 이 5B134977135E7D13 로 알려졌을 때

5B, 13, 49, 77, 13, 5E, 7D, 13 으로 보고 0번 부터 각 10, 20, 30 으로 XOR 해주면

4B, 33, 79, 67, 33, 6E, 6D, 33 이다. 

ASCII 로 바꾸었을 때 K3yg3nm3  값이 나온다.

![key14](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/key14.PNG)