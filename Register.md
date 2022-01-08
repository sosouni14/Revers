## CPU(Central Processing Unit)

자체적으로 데이터를 저장할 방법이 없기에 메모리로 직접 데이터를 전송할 수 없다. 사용자들이 입력한 명령어를 해석하고 연산한 후 그 결과를 제어한다.

**구조**

- 산술논리연산장치(ALU)
 - 레지스터 세트(Register Set)
 - 제어 유니트(Control Unit)

---

![CPU](https://i.postimg.cc/d1yzJkF2/CPU.png)





## 제어 유니트(Control Unit)
입력 받은 명령어를 기억 장치로부터 명령어를 읽어온(인출) 후 데이터 연산을 위해 어떤 동작(방향, 통로 등)을 할지 결정하기 위해 명령어를 해독한다(해독). 이것을 분석 해독하여 각 장치(기억장치, 연산장치, 입출력장치)에 실행에 필요한 정보들의 전송 통로와 방향을 지정해준다. 또한 CPU내부 요소들과 시스템 구성 요소들의 동작 시간도 결정한다.



## 산술논리연산장치(ALU)

명령어를 실행하기 위해 연산을 수행하는 장치이다.
연산에 필요한 데이터를 레지스터에서 가져오고 연산 결과를 다시 레지스터로 보낸다.



## 내부버스
산술/논리장치와 레지스터 간의 데이터 전송을 위한 데이터 버스와 제어장치로부터 발생하는 제어신호이다.
입출력장치나 기억장치와 같은 외부장치에 데이터를 전송하기 위해 시스템 버스인 주소버스, 데이터 버스, 제어버스를 이용한다. 시스템 버스와 직접 연결되지 않아 버퍼 레지스터나 시스템 버스 인터페이스 회로를 통해 시스템 버스와 연결한다.



## AT&T
원천지/목적지 로 표시된다. 피연사자(a,b 등) 앞에 % 상수 앞에 $ 붙는다.



## IA-32레지스터

>  Intel Architecture -32

어셈블리 명령에서 많이 사용되는 인텔의 32비트 마이크로프로세서에서 이용하는 명령 집합 아키텍처, 레지스터이다.

디버깅 초급단계에 사용되는 Basic program execution register

 **Basic program execution register**


 - General Purpose Registers(범용 레지스터)
 - Segment Registers(세그먼트 레지스터)
 - Program Status and Control Register(프로그램 상태와 컨트롤 레지스터)
 - Instruction Pointer



## Register(레지스터) - 일시적 저장 기억장치
CPU와 연결되어 있어서 연산속도가 빠른 고속메모리라고 불린다. 하지만 일시적으로 저장되므로 휘발성 메모리다. 특정 주소를 가리키거나 값을 읽기가 가능하다.



# 범용 레지스터(General Purpose Registers)



![EAX](https://i.postimg.cc/zXj4DYsc/Register.png)

​									상수 /주소 등을 주로 저장할때 사용. 크기 = 32bit(4byte)

---

![AX](https://i.postimg.cc/jdZk0QNV/EAX.png)

32bit 모두 사용시 – EAX /EBX /ECX /EDX

16bit 사용시 - AX

8bit 사용시 - AH(상위), AL(하위)로 사용 가능.

상황에 맞게 레지스터를 원하는 bit를 사용

---



**EAX(Extended[길어진] Accumulator Register)**

 - 범용 레지스터의 8개 레지스터 중 1개인 Accumlator. 산술 논리 연산 명령때 주로 EAX 레지스터를 사용하여 계산을
   하고 리턴 값을 저장한다. 특화된 연산은 EAX 내에서만 수행한다.
   
   호출한 함수가 연산 된 후 저장을 한다. 저장한 결과값(리턴 값)이 EAX에 기록한다.
   
   모든 함수의 리턴값을 EAX 레지스터를 사용해서 전달하고 저장되므로 호출 함수의 성공, 실패를 쉽게 알 수 있다.

---

**EDX**

 - Data. EAX와 역할이 같지만 리턴 값의 용도로 사용되지 않는다.
   
   산술 논리 연산 명령에도 이용된다. EDX가 사용될때는 EAX만 사용하기 힘들때 복잡한 연산 같은 경우 EDX가 덤으로 같이
   사용된다.

---

**ECX**

 - 루프문(Loop) 수행할때 카운팅 하는 역할
 ECX에 양수값을 넣고, 감소시키며 카운터가 0이 될 때까지 루프를 돈다.
    카운팅 할 필요가 없을 때는 변수로 사용한다.

---

**EBX**

 - Base. EAX, EDX, ECX 가 부족할때 사용되고 공간이 필요할때 예비 용도이다.

---

**ESI**

 - 데이터를 조작하거나 복사시에 ' 소스 데이터 ' 의 주소가 저장된다.
   
   ESI 레지스터가 가리키는 주소의 데이터를 EDI 레지스터가 가리키는 주소로 메모리 복사하는 용도로 많이 사용된다.

---

**EDI**

 - 데이터를 조작하거나 복사시에 ' 목적지 ' 의 주소가 저장된다.
   
   ESI 레지스터가 가리키는 주소의 데이터가 복사될 곳의 주소이다.
   
---


**EBP(Extended Base Pointer)**
 - 하나의 스택 프레임의 시작 지점 주소를 저장한다.
   
   스택 프레임의 베이스 주소(스택의 가장 아랫부분, 스택의 마지막) 저장
   
   EBP 이용하여 스택 내에서 함수 호출


---





> 참고 사이트
> [레지스터1](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=jaeyoon_95&logNo=221053588562)
> [레지스터2](https://sangcho.tistory.com/entry/%EC%A4%91%EC%95%99%EC%B2%98%EB%A6%AC%EC%9E%A5%EC%B9%98CPU )