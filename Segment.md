## Segment Registers(세그먼트 레지스터) - 메모리의 한 영역에 대한 주소 지정 제공

 - 큰 메모리를 조각 내어 그 조각마다 시작 주소, 범위, 접근 권한 등을 부여해서 메모리를 보호하는 기법이다.

 

> - 메모리 보호 기법: 자신에게 할당되지 않은 영역의 메모리에 접근하는 것을 막아 제어하는 방법이다.
> ---
> - 메모리 보호기법의 종류
>
> 1. ASLR(Address Space Layout Randomization)
> 2. DEP/NX(Data Execution Protection/Non-Executable)
> 3. ASCII-Armor
> 4. Canary


---
**CS(Code Segment)**

 - -함수나 제어문 같은 명령어들이 저장되는 코드 세그먼트 와 시작 주소를 가리킨다.
   
   레지스터의 오프셋 값을 더하면 실행하기 위해 메모리로부터 가져와야 할 명령어 주소

---
**SS(Stack Segment)**

 - 스택의 주소를 지정. 주소와 데이터를 일시적으로 저장할 목적으로 사용
   
   실행 과정에서 필요한 데이터, 연산 결과를 임시로 저장하거나 삭제할 때 사용하는 스택 세그먼트의 시작 주소

---
**DS(Data Segment)**

 - 전역, 정적 변수 데이터가 들어있는 데이터 세그먼트
   
   지정된 주소 값에 데이터의 오프셋을 더해 데이터 세그먼트 내에 위치해 있는 데이터 주소를 참조

---

**그외 추가적인 세그먼크**

- ES, FS, GS



---



## Program Status and Control Register(프로그램 상태와 컨트롤 레지스터)

- EFLAGS(Extended FLAGS): 플래그 레지스터라고 부르며 32bit 1개로 구성되어있다.

  각 비트는 0 또는 1로 값을 가진다. on/off  또는 true/false 를 뜻한다.

- 일부 비트는 시스템엣 직접 세팅하고 프로그램에서 사용된 명령의 수행 결과에 따라 세팅된다.

  - ZF(Zero Flag) : **연산 결과가 0이면 1로 되고 연산결과가 0이 아니면 0으로 셋팅**한다.

  - OF(Overflow Flag) : 부호 **있는** 수(Signed integer)의 **오버플로가 발생 시 1로 셋팅**한다.

  - CF(Carry Flag) : 부호 **없는** 수(Unsigned integer)의 **오버플로가 발생 시 1로 셋팅**한다.

    ⁙ 오버플로 : 정해놓은 메모리 영역을 넘어 다른 메모리 공간을 침범하는 것



----



## Instruction Pointer

- Instruction Pointer는 **CPU가 처리할 명령어의 주소를 나타내는 레지스터**이다.

  EIP에 저장된 메모리 주소의 명령어를 하나 처리하고 난 후 자동으로 그 명령어 길이만큼 EIP를 증가 시킨다.

- EIP(Extended Instruction Pointer) : 값을 **직접 변경이 불가능**하여 다른 명령어를 통하여 간접적으로 변경이 가능하다.




> 참고 사이트
> [메모리 보호 기법](https://horizon-pen.tistory.com/5)
>
> [레지스터](https://blog.naver.com/hungjaksm/40201226244)