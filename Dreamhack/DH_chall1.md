# DH_chall1

![chall1_1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_1.PNG)

Dreamhack 의 chall1 파일을 열었을 때 보이는 첫 화면이다.

전체적 흐름이 chall0 과 비슷해 보인다.

---

### call    sub_1400013E0

![chall1_2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_2.PNG)

`call    sub_1400012B0` 으로 들어갔다.



![chall1_3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_3.PNG)

`printf` 가 보인다. 메인 함수안에 있는 printf 라고 생각된다.

---

### call    sub_140001440

![chall1_4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_4.PNG)

`call    sub_140001300` 까지 들어가보았다.

![chall1_5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_5.PNG)

scanf 가 보이며 입력 값을 받는 부분이라고 생각된다.

결론은  `call    sub_1400013E0` 은 Input 을 불러오는 것이며,

`call    sub_140001440` 은 scanf 입력 값을 받게하여 입력 값을 저장한다.

---

### call    sub_140001000

![chall1_6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_6.PNG)

`call    sub_140001000` 에 들어가면 바로 보이는 부분들이다.

입력 값을 가지고 있는 rax 와 43인 C 와 비교를 한다.

만약 비교했을 때 값이 같을 경우 140001023 으로 점프한다는 조건식이다.

직접 실행해서 입력 값에 abcdefg 를 넣어보았다.

---

![chall1_8](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_8.PNG)

0번째 입력 값이 같지 않으므로 비교문에서 바로 나오게 되면서 Wrong 메시지가 띄어졌다.

그러면 0번째 값을 43인 C 값을 넣어보겠다.

![chall1_9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_9.PNG)

0번째 값에 C 을 넣었더니 다음 조건문으로 넘어가졌다.

1번째 입력 값을 a 를 넣었다.



![chall1_10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_10.PNG)

입력 값에 Cab 로 넣은 것이 보이며, 1 번째 입력 값 a 와 6F 인 o 와 비교하는 부분이 보인다.

1 번째 값이 다르게 써서 Wrong 이 나왔다. 이렇게 매번 그 자리의 값을 하나씩 비교하여 다 맞을 경우 Correct 메시지를 띄운다는 것을 알 수 있다.

비교문에 있는 값들을 하나 씩 다 적을 경우

**Compar3_the_ch4ract3r** 인  것을 알 수 있다. 이것을 입력 값에 다시 넣어서 실행해보겠다.

---

![chall1_11](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/chall1_11.PNG)

모든 비교문을 지나서 Correct 메시지가 띄어지는 것을 확인 할 수 있다.