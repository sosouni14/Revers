# Chall5(Dreamheak)

![chall512](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall512.PNG)

```c
for ( i = 0; (unsigned __int64)i < 0x18; ++i )
  {
    if ( *(unsigned __int8 *)(a1 + i + 1) + *(unsigned __int8 *)(a1 + i) != byte_7FF661EE3000[i] )
      return 0i64;
  }
  return 1i64;
```

```c
for ( i = 0; i < 24; ++i )
  {
    if ( a[i] + 1) + a[i] != byte_7FF661EE3000[i] )
      return 0i64;
  }
  return 1i64;
```



---

Input 값에 abcd 를 넣어보았다.

![chall513](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall513.PNG)

`movzx   eax, byte ptr [rcx+rax]` **RAX 61** a = 61 값이 들어갔다.



![chall514](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall514.PNG)

`inc     ecx`  **RCX 1** 값이 1 증가 되었다.



![chall515](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall515.PNG)

`movzx   ecx, byte ptr [rdx+rcx]` **RCX 62** b = 62 인 두번째 값이 들어갔다.



![chall516](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall516.PNG)

`add     eax, ecx`  **RAX C3** 이 되었다. RAX + RCX = 61 + 62 = 63 = C3 = RAX



![chall517](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall517.PNG)

`movzx   ecx, byte ptr [rdx+rcx]` **RCX AD** 로 바뀌였다.

Input 값이 달라져도 마지막 **RCX AD**는 바뀌지 않았다. 암호화 된 부분을 가져오는 것을 알 수 있다.



```c
a[i] + a[i+1] = i

a[0] + a[0+1] = AD
a[1] + a[2] = D8
    .
    .
    .
a[21] + a[22] = 98
a[22] + a[23] = 4c
a[23] + a[24] = 00
    
a[24] = 00
a[23] + 00 = 00 , a[23] = 00

a[22] + 00 = 4C

a[0] 순서대로 입력하면
41 6c 6c 5f 6c 31 66 65 5f 33 6e 64 73 5f 77 31 74 68 5f 4e 55 4c 4c 00
All_l1fe_3nds_w1th_NULL(공백)
이 나온다.
```

