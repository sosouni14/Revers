# chall3(Dreamheak)

파일을 열었을 때 처음 보이는 화면이다.

![chall33](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall33.PNG)

파일을 실행 시 보이는 화면이다.

![chall34](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall34.PNG)

입력 값을 받고 그 후 Correct, Wrong 을 띄우는 것을 예측해볼 수 있다.

---

### Call sub_7FF7D48A1000

![chall35](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall35.PNG)

디컴파일을 하였을 때 보이는 부분이다.  `if ( (unsigned int)sub_7FF7D48A1000((__int64)v4) )` 로 되어 있어 어떠한 내용이 있는지 다시 디컴파일하여 들어간다.

![chall36](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall36.PNG)

`sub_7FF7D48A1000` 이 디컴파일이 되었다. for 문에 if 문이 있는 것을 보아서 반복 조건문인 걸 알 수 있다.

```c++
for ( i = 0; (unsigned __int64)i < 0x18; ++i )
  {
    if ( byte_7FF7D48A3000[i] != (i ^ *(unsigned __int8 *)(a1 + i)) + 2 * i )
      return 0i64;
  }
  return 1i64;
```

이 부분을 아는 부분만 먼저 해석해 보겠다.

```c++
for ( i = 0; i < 24; ++i )
    {
    if ( byte_7FF7D48A3000[i] != (i ^ X + 2 * i )
      return 0i64;
  }
  return 1i64;
```

이렇게 해석할 수 있을 것 같다. `byte_7FF7D48A3000` 무엇인지  확인한다.

---

#### byte_7FF7D48A3000

![chall32](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall32.PNG)

16진수 값들이 들어져있다.

---

### Call sub_7FF7D48A1000

![chall37](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall37.PNG)

디컴파일 하지 않고 `Call sub_7FF7D48A1000` 에 들어가보았다.

반복문 부분이라는 것을 확실히 알 수 있다.

여기서부터 디버깅을 시작해본다.

---

![chall41](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall41.PNG)

**Input : aaa** 입력 값을 넣고 디버깅을 하는데 `RAX = 0` 이 

`movzx   eax, byte ptr [rcx+rax]` 값이 복사되면서 **RAX 49** 가 되었다.

`movzx   ecx, byte ptr [rdx+rcx]` **RCX 61** 이 되었다.

하지만 값이 옳지 않아 반복문에서 나와지고 Wrong 부분으로 점프되었다.

무슨 값을 넣어도 `movzx   eax, byte ptr [rcx+rax]` 에서 **RAX 49** 가 되었다.

그래서 if문 에서 보았던 값들이 플래그라고 생각되었다.

하지만 49, 60, 67 를 ASCII 로 바꾸고 그 값을 Input 하였을 때 1번만 반복문에서 다시 실행되는 것을 보고

if문의 `7FF7D48A3000` 은 암호화로 바꾸어진 값이라고 예상해볼 수 있다. 

---

### 49 = I

![chall39](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall40.PNG)

49 인 ASCII 값인 I 를 `Input`  에 `Ia` 를 입력했다. 

`movzx   eax, byte ptr [rcx+rax]` **RAX 49** 이며,

`movzx   ecx, byte ptr [rdx+rcx]` **RCX 49** 가 된 것을 확인할 수 있다.

`cmp     eax, ecx `  **RAX** 는 비교 값이며 **RCX** 는 입력 값이라고 예측해볼 수 있다.

`jz      short loc_7FF7D48A1051` 값이 동일하기에 `loc_7FF7D48A1051` 로 갈 수 있다.

`jmp     short loc_7FF7D48A1012` 로 점프하면 다시 반복문으로 들어가진다.



![chall42](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall42.PNG)

**RAX = 1** 값이 증가되었다.



![chall43](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall43.PNG)

`movzx   eax, byte ptr [rcx+rax]` **RAX 60** 이다.



![chall44](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall44.PNG)

`movsxd  rcx, [rsp+18h+var_18]` **RCX 1**  이 되었다.



![chall45](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall45.PNG)

`movzx   ecx, byte ptr [rdx+rcx]` 에서 **RCX 61** 이 되었다.



![chall46](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall46.PNG)

`xor     ecx, [rsp+18h+var_18]` 에서 **RCX 60** 이 되었다.



![chall47](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall47.PNG)

`mov     edx, [rsp+18h+var_18]` 에 **RDX 1** 이 되었다.



![chall48](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall48.PNG)

`lea     ecx, [rcx+rdx*2]` **RCX 62** 가 되며,

`cmp     eax, ecx` 에서 서로 다른 값이므로 반복문이 종료되면서 Wrong 부분으로 점프된다.

그래서 이 부분을 다시 디컴파일 하였다.



![chall36](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall36.PNG)

`7FF7D48A3000` 에 들어간 문자하고 같지 않으면 반복문이 종료되는 것을 확실히 알게 되었고 입력값을 받으면 그것을 암호화해서 들어간 것이 `7FF7D48A3000` 을 알 수 있게 되었다.

```c++
byte_7FF7D48A3000[i] != (i ^ X + 2 * i )

i = 반복 횟수
    i XOR X + 2 * i
    0 XOR X + 2 * 0 = 49, x = 49;

i = 1
    1 XOR X + 2 * 1 = 60
    60 - 2 = 5E XOR 1 = 5F = X;

.
.
.
 
i = 10 = A
    10 XOR X + 2 * 10 = 69
    A XOR X + 20 = 69
    A XOR X + 14 = 69 // 16진수로 바꿔준다.
    69 - 14 = 55 XOR A = 5F
```

이렇게 식으로 23개의 16진수 값을 구하였다.

![chall49](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall49.PNG)

0부터 시작하여 23까지 구해진 16진수 값은 

49 5F 61 6D 5F 58 30 5F 78 6F 5F 58 6F 72 5F 65 58 63 69 74 31 6E 67 00 이며

ASCII 값은 

I_am_X0_xo_Xor_eXcit1ng 이다.

![chall50](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall50.PNG)

Correct 부분에 들어온 것을 확인할 수 있다.