# Chall4(Dreamhack)

![chall413](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall413.PNG)

![chall414](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall414.PNG)

파일을 열고 실행했을 때 보이는 화면들이다.



---

### call    sub_7FF7CD7C1000

![chall415](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall415.PNG)

![chall416](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall416.PNG)

for 문과 if문이 있는 것을 확인할 수 있다.



---

### byte_7FF7CD7C3000

![chall411](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall411.PNG)

조건문에 사용되는 값들을 확인할 수 있다.



---

### sub_7FF7CD7C1000

![chall418](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall418.PNG)

![chall417](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall417.PNG)

`movzx   eax, byte ptr [rcx+rax]` **RAX 61** 이 된 것을 확인 할 수 있다.



![chall419](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall419.PNG)

`sar     eax, 4` **RAX 6** 이 되었다.  `sar` 쉬프트 연산자로 4비트만큼 오른쪽으로 쉬프트 연산을 하겠다는 뜻이다. 그래서 61 인 16진수를 2진수로 바꾸었을 때 0110 0001 인데 이것을 쉬프트 연산을 하였을 때 6(0110) 이 나온다.



![chall420](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall420.PNG)

`movzx   ecx, byte ptr [rdx+rcx]` **RCX 61** 이 들어가졌다.



![chall421](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall421.PNG)

`shl     ecx, 4` **RCX 610** 이 되었다. `shl` 쉬프트 연산자로 4비트만큼 왼쪽으로 쉬프트 연산을 하겠다는 뜻이다. 61 = 0110 0001 를 왼쪽으로 쉬프트 연산하면 610 = 0110 0001 0000 으로 나온다.



![chall422](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall422.PNG)

`and     ecx, 0F0h` **RCX 10** 이 되었다. 610 = 0110 0001 0000 & F0 = 1111 0000 이므로 0001 0000 = 10 이 나오는 것이다.



![chall423](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall423.PNG)

`or      eax, ecx` **RAX 16**이 되었다. RAX = 0110 or RCX = 0001 0000, RAX = 0001 0110 = 16 이 나온다.



![chall424](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall424.PNG)

`movzx   ecx, byte ptr [rdx+rcx]`  **RCX 24** 가 되었다.  `byte_7FF7CD7C3000` 에 있는 값이라고 예측할 수 있다.

`cmp     eax, ecx` 부분에서 값이 서로 같지 않으므로 Correct  부분에 들어가지 못하였다.



이것을 프로그래밍을 해볼 수 있다.

```c
#include <stdio.h>
#include <string.h>

int main()
{
    char arg[28] = { 0x24, 0x27, 0x13, 0xC6, 0xC6, 0x13, 0x16, 0xE6, 0x47, 
        0xF5, 0x26, 0x96, 0x47, 0xF5, 0x46, 0x27, 0x13, 0x26, 0x26, 0xC6, 0x56, 0xF5, 0xC3, 0xC3, 0xF5, 0xE3,0xE3 };
    for(int i = 0; i < 28; i++)
    {
        char sr = (arg[i] & 0xF0 >> 4);
        char sl = (arg[i] & 0x0F << 4);
        printf("%c", sl | sr);
    }
}
```



**OR**

![chall412](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall412.PNG)

![chall425](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall425.PNG)

이 부분을 보고 해석해볼 수도 있다. 밑에 사진은 Pro로 열었을 때 보이는 부분이다.



```c
원문
for ( i = 0; (unsigned __int64)i < 0x1C; ++i )
  {
    if ( (16 * *(_BYTE *)(a1 + i) & 0xF0 | ((signed int)*(unsigned __int8 *)(a1 + i) >> 4)) != byte_140003000[i] )
      return 0i64;
  }
  return 1i64;

프로그래밍
int main()
{
    int a[28] = {0x24, 0x27, 0x13, 0xC6, 0xC6, 0x13, 0x16, 0xE6, 0x47, 
        0xF5, 0x26, 0x96, 0x47, 0xF5, 0x46, 0x27, 0x13, 0x26, 0x26, 0xC6, 0x56, 0xF5, 0xC3, 0xC3, 0xF5, 0xE3,0xE3};
    for(int i = 0; i <28; i++)
    {
        printf("%c", (16 * a[i] & 0xF0) | (a[i] >> 4));
    }
    return 0;
}
```

![chall426](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall426.PNG)

이렇게 나오는 것을 확인할 수 있다.
