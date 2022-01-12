## Chall1(Dreamhack)

![chal1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal1.PNG)

파일을 열면 첫번째로 보이는 화면이다.

---

## Call sub_7FF69BA813E0

`Call sub_7FF69BA813E0` 에 들어가면 `call __acrt_iob_func` 와 `sub_7FF69BA812B0` 이 있는 것을 하나 씩 확인해본다.

#### Call __acrt_iob_func

![chal3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal3.PNG)

간접 호출 cdecl을 확인 할 수 있다.

---

#### Call sub_7FF69BA812B0

![chal4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal4.PNG)

`call sub_7FF69BA81290` 와 `call __stdio_common_vfprintf` 가 보인다.

##### call __stdio_common_vfprintf

![chal6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal6.PNG)

`printf` 이므로 `input` 을 출력하는 부분이라고 예측해본다.

---

## Call sub_7FF69BA81440

![chal7](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal7.PNG)

`Call sub_7FF69BA81440` 에 들어가면 보이는 화면이다.

`call __acrt_iob_func` 와 `call sub_7FF69BA81300` 불러오는 것을 확인할 수 있다.

#### call __acrt_iob_func

![chal8](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal8.PNG)

간접 호출 cdecl을 확인 할 수 있다.

---

#### call sub_7FF69BA81300

![chal9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal9.PNG)

`call sub_7FF69BA81300` 에 들어가면 보이는 화면이다.

`call sub_7FF69BA812A0` 와 `call __stdio_common_vfscanf` 를 확인할 수 있다.

##### call __stdio_common_vfscanf

![chal10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal10.PNG)

scanf를 확인 할 수 있다. 입력 값 부분인 것을 알 수 있다.

---

## call sub_7FF69BA81000

![chal11](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal11.PNG)

`call sub_7FF69BA81000` 을 들어가면 보이는 화면이다.

비교부분이 보이면서 조건을 확인하면서 점프하겠다는 것이 보인다.

`cmp eax, 43h ; 'C'`을 보면 입력 값과 원래 값을 비교하는 것을 확인 할 수 있다.

이 부분을 하나씩 다 나열해서 보면 **Compar3_the_ch4ract3r** 이다.

![chal12](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chal12.PNG)

**Compar3_the_ch4ract3r** 을 `input`에 입력 하면 **Correct 문**으로 넘어가는 것을 확인할 수 있다.



> Dreamhack