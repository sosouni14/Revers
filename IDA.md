## IDA

- ##### 단축키

  **함수 및 변수 이름 재설정**

  N : 함수 및 변수 이름을 재설정 가능하다. 정의되지 않은 함수 및 변수의 경우 해당 기능을 통해 이름을 설정하여 분석 속도를 향상시킬 수 있다.

  

  **Cross reference (Xref)**

  X :  해당 함수 및 변수가 사용되는 영역을 재참조할 수 있다.

  

  **함수 및 변수 타입 변경**

  Y : 해당 함수 및 변수의 타입을 지정할 수 있다. 함수의 경우, 전달되는 매개 변수를 추가하거나, 타입을 변경할 수 있다.



​		**Strings**

​		Shift + F12 :  바이너리에서 사용하는 모든 문자열을 조회할 수 있다.

​		함수의 심볼이 존재하지 않거나, 복잡한 경우 문자열을 통해 분석 시간을 크게 단축 할 수 있다.

​		

​		**Decompile**

​		어셈블리를 C언어 형태로 변환하여 보여준다.

---

## sub_140001060

함수의 디컴파일 결과를 살펴보면 `va_start` 함수를 통해 가변 인자를 처리하는 함수임을 알 수 있다.

`__acrt_iob_func` 함수는 스트림을 가져올 때 사용되는 함수인데, 인자로 들어가는 1은 `stdout` 을 의미한다. 따라서 문자열 인자를 받고 `stdout` 스트림을 내부적으로 사용하는 가변 함수임을 알 수 있다.

`sub_140001060` 함수는 `printf` 함수로 추정할 수 있다.

> 스트림이란?
>
> 스트림(Stream) 은 데이터가 조금씩 흘러들어온다는 의미에서 명명되었다.
>
> 운영체제는 *stdin (standard input), stdout (standard output), stderr (standard error)* 와 같은 기본 스트림들을 프로세스마다 생성해줍니다. 이들은 일반적으로 사용자와 프로세스를 연결해주기 위해 사용됩니다. *printf* 함수는 *stdout* 을 통해 출력 데이터를 우리가 볼 수 있게 해주며, *scanf* 함수는 우리의 키보드 입력을 *stdin*으로 받아서 프로세스에 전달해 준다.

---

## 동적 분석

##### 중단점 설정(Breack Point, F2) 및 실행(Run F9)

38h 에서 h = hex 이다.

kernel32 는 dll 이 ida 로 확인하기 어렵기 때문에 kernel32가 무엇을 하는지 확인하기 어렵다.

동적 분석을 통해 call sub_140001060이 printf 인 것을 알았다.

---

## 함수 내부로 진입하기(Step Into, F7)

Step Over(F8)가 함수의 내부로 진입하지 않는다는 것을 발견할 수 있다.

Step Into(F7)은 함수 내부 진입이 가능하다.

> StackCookie : 원래 있는 StackCookie 와 생성된 StackCookie 을 비교하는데
>
> 비교할때 값이 같다면 종료되지만 그렇지 않을 경우 종료되지 않고 그곳에서 멈춘다.

Call 함수 안으로 들어갈 경우 RIP 또한 내부로 들어간 것을 확인 할 수 있다.

---

## Appendix, 실행 중인 프로세스 조작하기

IDA를 이용하면 실행중인 프로세스의 메모리를 조작할 수 있다.

스택에서 해당 값을 클릭하고 F2를 누른뒤 원하는 값을 적은 후 F2를 다시 눌러서 실행할 경우 그 값만큼 멈춰있다가 다음 명령문으로 넘어간다.

---

