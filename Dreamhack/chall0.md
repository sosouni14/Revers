## chall0 (Dreamhack)

chall0.exe 를 실행했을 때 보이는 창이다.

![chall1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1.PNG)

security_cookie 가 있다는 것은 이미 생성된 값이 있어 비교할 부분 있다고 예상할 수 있다.



**F5** 를 눌러서 디컴파일된 부분을 확인해보니 메인 부분을 확인 할 수 있었다.

![chall2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall2.PNG)

메인 함수 부분에서 input 과 입력 값 비교하여 "Correct", "Wrong" 부분을 출력하는 것이 보인다.

sub_7FF6EDE41190 은 input 값이 입력되어 있다.

sub_7FF6EDE411F0 은 v4가 적혀져있다. 

윗 `memset(v4,0,sizeof(v4));` 이 `v4`가 무엇인지 알려주지만 잘 모르겠다.

하지만 `if ( sub_7FF6EDE41000(v4) )` 을 예상하자면 만약 `v4`가 `x` 랑 같다면 으로 되기에

`v4` 는 입력 값이라고 예측할 수 있다. 

---

## call sub_7FF6EDE41190

![chall4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall4.PNG)

sub_7FF6EDE41190 으로 들어가면 `call __arct_iob_func` 함수가 보인다.



![chall5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall5.PNG)

간접호출 하는 cdecl을 보고 알 수 있다.

## Call sub_7FF6EDE41060

![chall7](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall7.PNG)

`call __stdio_common_vfprintf` 함수에 들어간다.

![chall8](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall8.PNG)

메인 함수 안에 있는 printf 함수인 것을 확실하게 알 수 있다.

---

## Call sub_7FF6EDE411F0

![chall6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall6.PNG)

`call _arct_iob_func` 을 들어가면 이전 call 과 같다.

![chall5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall5.PNG)

## Call sub_7FF6EDE410B0

![chall9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall9.PNG)

![chall10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall10.PNG)

`call __stdio_common_vfscanf` 입력 값에 `scanf` 쓰이는 것을 예측할 수 있다.

---

## Call sub_7FF6EDE41000

![chall11](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall11.PNG)

`call` 에 들어가니 조건식 있는 것을 확인할 수 있다.

![chall12](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall12.PNG)

`call strcmp` 함수는 문자열 비교하는 것이다. 고로 입력 값과 생성된 문자열을 비교한다는 것을 알 수 있다.

조건식과 비교 함수를 보고 "Compar3_the_str1ng" 이 input에 입력될 값인 것을 알 수 있다.

