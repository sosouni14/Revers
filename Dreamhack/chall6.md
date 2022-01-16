# chall6(Dreamhack)

![chall63](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall63.PNG)

chall6 을 열었을 때 보이는 화면이다.

`call    sub_7FF749481000` 에 들어간다.

---

```c
__int64 __fastcall sub_7FF749481000(__int64 a1)
{
  int i; // [rsp+0h] [rbp-18h]

  for ( i = 0; (unsigned __int64)i < 0x12; ++i )
  {
    if ( byte_7FF749483020[*(unsigned __int8 *)(a1 + i)] != byte_7FF749483000[i] )
      return 0i64;
  }
  return 1i64;
}
```

디컴파일을 하면 for문과 if문이 있다.

```c
for (i = 0; i < 18; < i++)
	{
		if(3020 a[i] != 암호화(3000))
       	 	return 0i64;
	}
```

이렇게 해석해보았다.



---

![chall64](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall64.PNG)

입력 값에 abcd를 입력하고 돌려보았다.

input : abcd / a = EF / b = AA / c = FB / 1 = C7

`cmp     rax, 12h`	12(16) = 18 반복

`jnb     short loc_7FF749481055`

`movsxd  rax, [rsp+18h+var_18]`

`mov     rcx, [rsp+18h+arg_0]`

`movzx   eax, byte ptr [rcx+rax]`	**RAX 61** 입력값

`lea     rcx, byte_7FF749483020`	**RCX 7FF749483020** 첫번째 암호화 값

`movzx   eax, byte ptr [rcx+rax]`	**RAX EF** 무작위 (rcx + rax = rax)

`movsxd  rcx, [rsp+18h+var_18]`	**RCX 00** 

`lea     rdx, byte_7FF749483000`	**RDX 7FF749483000**

`movzx   ecx, byte ptr [rdx+rcx]`	**RCX 00	RAX EF	RDX 7FF749483000**

`cmp     eax, ecx`



처음 디버깅을 하며 레지스터 값들을 적어보았다.

브루트포스 알고리즘 이 사용되었다는 것을 이미 알고 있는 상태였기에

입력값 + x(무작위) = 암호화 된거라고 생각하였다.

그리고 b, c, 0, 1 를 첫번째 값으로 넣었을 때 각 다른 값이 RAX 에 찍혔지만 RCX는 항상 00 인 것을 확인 할 수 있었다.

```
1. movzx   eax, byte ptr [rcx+rax]
2. lea     rcx, byte_7FF749483020
3. movzx   eax, byte ptr [rcx+rax]
```

여기 순서 부분에서 RAX 값이 달라져서 이 부분을 좀 더 돌려보면서 생각해보았다.

먼저 입력 값을 eax에 넣는 것이다. 그래서 a를 넣었을 때 61 이며 16진수로 표기 되었다.

![chall65](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall65.PNG)



2번째, 3020 에서 rcx 를 넣었을 때 3020 위치의 값으로 되는 것을 확인하였다.

![chall66](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall66.PNG)



3번째,  rcx + rax = eax 이다. x + 61 = EF 가 되었다.

![chall67](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall67.PNG)

이 부분에서 무슨 값을 넣을 때마다 각기 다른 값이 나오는 것을 확인하였다. 그래서 여기서 암호화로 만드는 것이라고 생각되었다.

`cmp     eax, ecx` 되기 전에 RAX = EF, RCX = 00 으로 비교를 하기 때문에

RAX = 3020 에 있는 위치 값을 가져오고

RCX = 3000 에 있는 암호 위치 값을 가져오는 것을 알 수 있다.



위치 값을 가져오는 것만 알고 어떤 형식으로 가져오는지 몰라서 

b 값 넣었을 때 AA, C 값 넣었을 때 FB 나오는데 

![chall68](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall68.PNG)

![chall69](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall69.PNG)



![chall70](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall70.PNG)

3020 에서 바로 옆에 값이 나오는 것을 확인 할 수 있었다.

그리고 0, 1, 2 를 넣었을 때 다른 n번째에서 0부터 순서대로 암호 값이 나오는 것을 레지스터를 보고 알 수 있었다.

![chall71](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall71.PNG)

![chall72](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall72.PNG)



![chall73](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall73.PNG)

a 랑 0이 왜 다른 위치 부분에 있지만 a 다음은 b 가 있고 0 다음에 1이 있다는 것을 보고 시작되는 값 순서대로 입력 되어 있는 것을 확인 할 수 있었다

여기서 a = 61, EF 라서 3020 의 61 번째 위치에 있다고 생각되어서 61번째를 세어서 보니 다른 값이 들어가 있었다.

그래서  EF는 몇 번째에 있는지 알기 위해서 세어보니 97번째 있는 것이다.

16진수 와 10진수 숫자는 다르므로 계산기에 16진수 61 를 써보니 10진수로 97 인 것을 확인할 수 있었다.

그래서 10진수로 97번째 있는 61 = a 인 것을 암호화에서 EF가 나오는 것을 알 수 있게 되었다.

이런 방식으로 00 으로 써져있는 암호화를 3020 에 몇번째에 적혀있는지 숫자를 세어보고 10진수로 세었던 것을 16진수 값으로 바꾸고 그것을 다시 ASCII 로 바꾸었을 때 R이 나오는 것을 확인할 수 있었다.



```c
3000에 암호화 첫번째 00 은 3020 위치에서 82번째 이며
16진수로는 52 이고 ASCII로 R 이다.

3000에 암호화 두번째 4D 는 3020 위치에 101번째 이며
16진수로는 65 이고 ASCIIfh e 이다.

마지막 63은 3020 위치에서 0번째 이므로
16진수로도 0 이므로 공백처리가 된다.    
```

이런 식으로 계산을 하였을 때 

**Replac3_the_w0rld** 가 나온다.

