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

`RAX = 1` 값이 증가되었다.



![chall43](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall43.PNG)

`movzx   eax, byte ptr [rcx+rax]` **RAX 60** 이다.

![chall44](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall44.PNG)

`movsxd  rcx, [rsp+18h+var_18]` **RCX 1**  이 되었다.

![chall45](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall45.PNG)