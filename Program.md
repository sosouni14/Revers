## 프로그램(Program)

프로그램(Program)은 연산 장치가 수행해야 하는 동작을 정의한 일종의 문서이다.

컴퓨터에서 프로그램이 저장 장치에 이진(Binary) 형태로 저장되기 때문에 바이너리(Binary)라고도 부른다.

---

## 컴파일(Compile)

cpu가 수행해야 할 명령어들을 프로그래밍 언어로 작성한 것을 **소스코드(Source Code)** 라고 한다.

이를 컴퓨터가 이행할 수 있는 기계어 형식으로 번역하는 것을 **컴파일(Compile)** 라고 한다.

소스코드를 어셈블리어로 번역하는 과정이다.

소프트웨어는 **컴파일러(Compiler)** 라고 하며 GCC, Clang, MSVC 등이 있다.

**Python, Javascript** 객체지향(Object-Oriented Code) OOC 프로그램은 컴파일을 필요로 하지 않는다.

---

## 전처리(Preprocessing)

소스 코드가 컴파일 에 필요한 형식으로 가공되는 과정이다.

주석 제거 → 매크로 치환 → 파일 병합

---

## 어셈블(Assemble)

어셈블리 코드를 기계어로 번역하고 실행 가능한 형식의 변형하는 과정이다.

---

## 링크(Link)

여러 개의 목적 파일을 하나로 묶고, 필요한 라이브러리와 연결해주는 과정이다.

---

## 디스어샘블(Disassemble)

바이너리를 어셈블리어로 번역하는 과정이다.

---

## 디컴파일(Decompiler)

바이너리를 고급 언어로 번역하는 과정이다.

---

## 정적 분석(Static Analysis)

장점 : 전체 구조를 파악하기 쉬우며 분석 환경의 제약에서도 비교적 자유롭다.

바이러스와 같은 악성 프로그램의 위협으로부터 안전하다.

단점 : 소스 코드를 알아보기 힘든 형태이거나 분석하기 힘들게 할 경우이다.

---

## 동적 분석(Dynamic Analysis)

장점 : 코드를 자세히 분석해보지 않아도 프로그램의 개략적인 동작을 파악할 수 있다.

단점 : 분석 환경을 구축하기 어렵다.

---

## 컴퓨터 구조(Computer Architecture)

컴퓨터가 효율적으로 작동할 수 있도록 하드웨어 및 소프트웨어의 기능을 고안하고 구성하는 방법이다.

---

## 명령어 집합구조(Instruction Set Architecture)

CPU 가 처리해야하는 명령어를 설계하는 분야이다.

---

## 마이크로 아키텍처(Micro Architecture)

명령어 집합을 효율적으로 처리할 수 있도록 CPU의 회로를 설계하는 분야이다.

---

## 폰 노이만 구조

##### 중앙처리장치(Central Processing Unit)

산술/논리 연산을 처리하는 **산술논리장치** 와 CPU를 제어하는 **제어장치**, CPU에 필요한 데이터를 저장하는 **레지스터 등**으로 구성된다.



##### 기억장치(주기억 장치, 보조기억장치)

**주기억장치** 로 임시로 저장되는 램(Random-Access Memory, RAM)이 있다.

**보조기억장치** 운영체제, 프로그램 등과 같은 데이터를 장기간 보관하고자 할 때 사용된다.(HDD,SSD)



##### 버스(BUS)

부품과 부품사이 또는 신호를 전송하는 통로를 말한다.

---

## x86-64 아키텍처

##### n 비트 아키텍처

CPU가 이해 할 수 있는 데이터의 단위이다.(WORD)

CPU는 32bit의 데이터만 처리할 수 있다.

WORD가 크면 유리한 부분은 소프트웨어의 최고 성능과 실행이 불가능한 상황이 없기때문이다.

#### 범용 레지스터(Gemeral Register)

| 이름                    | 주용도                                    |
| ----------------------- | ----------------------------------------- |
| rax(accumlator)         | 함수의 반환 값                            |
| rbx(base)               | x64에서는 주된 용도 없다.                 |
| rcx(counter)            | 반복문의 반복 횟수, 각종 연산의 시행 횟수 |
| rdx(data)               | x64에서는 주된 용도 없다.                 |
| rsi(source)             | 데이터를 옮길 때 원본을 가리키는 포인터   |
| rdi(destination)        | 데이터를 옮길 때 목적지를 가리키는 포인터 |
| rsp(stack pointer)      | 사용중인 스택의 위치를 가리키는 포인터    |
| rbp(stack base pointer) | 스택의 바닥을 가리키는 포인터             |

#### 세그먼트 레지스터(Segment Register)

**cs, ss, ds, es, fs, gs** 총 6가지 세그먼트 레지스터가 있다. 각 레지스터의 크기는 16bit 이다.

**cs, ds, ss** 레지스터는 코드 영역과 데이터, 스택 메모리 영역을 가리킬 때 사용된다.



#### 명령어 포인터 레지스터(Instruction Pointer Register, IP)

CPU가 어느 부분의 코드를 실행할지 가리키는게 명령어 포인터 레지스터의 역할이다.

x64 아키텍처의 명령어 제지스터는 **rip** 이며 크기는 **8byte** 이다.

---

## 플래그 레지스터(RFLAGS)

CPU의 상태를 저장하는 레지스터이다.

| 플래그            | 의미                                                         |
| ----------------- | ------------------------------------------------------------ |
| CF(Carry Flag)    | 부호 없는 수의 연산 결과가 비트의 범위를 넘을 경우 설정된다. |
| ZF(Zero Flag)     | 연산의 결과가 0일 경우 설정된다.                             |
| SF(Sign Flag)     | 연산의 결과가 음수일 경우 설정된다.                          |
| OF(Overflow Flag) | 부호 있는 수의 연산 결과가 비트 범위를 넘을 경우 설정된다.   |

---

- rax= 0x123456789abcdef 일때 eax, ax, ah, al 값은?
  1. eax = 0x89abcdef = 32bit
  2. ax = 0xcdef = 16bit
  3. ah = 0xcd = 8bit
  4. al = 0xef = 8bit

- rax 에서 rbx를 뺐을 때 ZF 가 설정 될 경우

  rax == rbx 경우이어야 0이 되며 ZF가 된다.

---

## 메모리 레이아웃(Memory Layout)

프로그램을 실행하면 운영체제는 프로세스에게 메모리 공간을 할당해준다. 그것을 가상 메모리(Virtual Memory) 라고 한다.

프로세스가 사용하는 데이터를 적절한 구획에 저장하며 유사한 데이터를 모아놓기 때문에 운영체제는각 구획에 적절한 권한을 부여할 수 있으며  개발자는 프로세스의 메모리를 더 직관적으로 이해할 수 있다.

---

## 섹션

유사한 용도로 사용되는 데이터가 모여있는 영역이다. 

".text" 섹션에는 PE의 코드가 적혀있고, ".data" 에는 PE가 실행중에 참조하는 데이터가 적혀있다.

섹션에 대한 정보는 PE헤더에 적혀있다. 

- 섹션의 이름
- 섹션의 크기
- 섹션이 로드될 주소의 오프셋
- 섹션의 속성과 권한

---

dos header 64byte

e_magic: dos signature 

dos stub - optional

크기도 가변적이다. e_lfanew 값을 이용 한다는게 무슨말?

코드와 데이터가 함께 구성된다.

이미지 엔티 헤더



pe헤더 - nt header - file header

속성을 표시. 

charactices



imagebase - eip = imagebase + addressofentrypoint

numberofrvaandsizes ??? what?

---

## .text

실행 가능한 기계 코드 가 위치하는 영역이다. 읽기 권한과 실행 권한이 부여된다.

쓰기 권한이 없는 이유는 공격자가 악의적인 코드를 삽입하기 쉬워지므로 운영체제는 이 세그먼트에 쓰기 권한을 제거한다.

---

## .data

컴파일 시점에 값이 정해진 전역 변수들이 위치한다.

CPU가 이 섹션의 데이터를 읽고 쓰기를 하기 위해 읽기/ 쓰기 권한을 준다.

- 초기화된 전역 변수, 전역 상수

---

## .rdata

컴파일 시점에 값이 정해진 전역 상수와 참조할 DLL 및 외부 함수들의 정보가 저장된다.

CPU가 섹션의 데이터를 읽을 수 있어야 하므로, 읽기 권한이 부여되지만 쓰기는 불가능하다.

소켓에 주로 사용되는 str_ptr 에서

str_ptr 은 "readonly" 라는 문자열을 가리키고 있다. 여기서 **str_ptr은 전역 변수**로 **.data** 에 위치 하지만

"readonly" 는 **상수 문자열**로 취급되어 **.rdata** 에 위치한다.

- 전역 상수, 임포트 데이터

---

## 스택(Stack)

지역 변수 나 함수의 리턴 주소가 저장된다. 읽기/ 쓰기 권한이 부여된다.

스택이 확장될 때 기존 주소보다 낮은 주소로 확장된다.

- 지역 변수, 함수의 인자 등

---

## 힙(Hip)

프로그램이 여러 용도로 사용하기 위해 할당받는 공간이다. 그러므로 모든 종류의 데이터가 저장 가능하다.

스택보다 큰 데이터를 저장 가능하며, 전역적으로 접근이 가능하다. 또한 실행 중 동적으로 메모리 할당 받는다.

위치가 정확한다면 원하는 만큼 메모리 할당을 받을 수 있다.

- malloc(), calloc() 등으로 할당 받은 메모리

---

## x64 어셈블리 언어

##### 기본 구조

명령어(Operation Code, Opcode)와 목적어에 해당되는 피연산자(Operand)로 구성된다.

| opcode | operand1 | operand2 |
| ------ | -------- | -------- |
| mov    | eax      | 3        |

##### 명령어

| 명령 코드                   |                    |
| --------------------------- | ------------------ |
| 데이터 이동 (Data Transfer) | mov, lea           |
| 산술 연산 (Arithmetic)      | inc, dec, add, sub |
| 논리 연산 (Logical)         | and, or, xor, not  |
| 비교 (Comparison)           | cmp, test          |
| 분기 (Branch)               | jmp, je, jg        |
| 스택 (Stack)                | push, pop          |
| 프로시져 (Procedure)        | call, ret, leave   |
| 시스템 콜 (System Call)     | syscall            |

---

## 피연산자

- 상수 (Immediate Value)
- 레지스터 (Register)
- 메모리 (Memory)

##### 메모리 피연산자의 예

| 메모리 피연산자       |                                                    |
| --------------------- | -------------------------------------------------- |
| QWORD PTR [0x8048000] | 0x8048000의 데이터를 8바이트만큼 참조              |
| DWORD PTR [0x8048000] | 0x8048000의 데이터를 4바이트만큼 참조              |
| WORD PTR [rax]        | rax가 가리키는 주소에서 데이터를 2바이트 만큼 참조 |

> PTR : ip 주소에 대해 도메인명을 매핑하여 주며, 역방향 도메인 파일에서 사용된다.

---

## 데이터 이동

어떤 값을 레지스터나 메모리에 옮기도록 지시한다.

| mov dst, src : src에 들어있는 값을 dst에 대입 |                                             |
| --------------------------------------------- | ------------------------------------------- |
| mov rdi, rsi                                  | ris의 값을 rdi에 대입                       |
| mov QWORD PTR[rdi], rsi                       | rsi의 값을 rdi가 가리키는 주소에 대입       |
| mov QWORD PTR[rid+8*rcx], rsi                 | rsi의 값을 rdi+8*rcx가 가리키는 주소에 대입 |

| lea dst, src : src의 유효 주소(Effective Address, EA)를 dst에 저장한다. |                         |
| ------------------------------------------------------------ | ----------------------- |
| lea rsi, [rdx+8*rcx]                                         | rdx+8*rcx 를 rsi에 대입 |

---

## 산술 연산

덧셈, 뺄셈, 곱셈, 나눗셈 연산을 지시한다.

| add dst, src : dst에 src의 값을 더한다. |                    |
| --------------------------------------- | ------------------ |
| add eax, 3                              | eax += 3           |
| add ax, WORD PTR[rdi]                   | ax += *(WORD *)rdi |

| sub dst, src : dst에서 src의 값을 뺀다. |                    |
| --------------------------------------- | ------------------ |
| sub eax, 3                              | eax -= 3           |
| sub ax, WROD PTR[rdi]                   | ax -= *(WORD *)rdi |

| inc op : op의 값을 1 증가 시킨다. |          |
| --------------------------------- | -------- |
| inc eax                           | eax += 1 |

| dec op : op의 값을 1 감소 시킨다. |          |
| --------------------------------- | -------- |
| dex eax                           | eax -= 1 |

---

## 논리 연산 and & or

and, or, xor, neg 등의 비트 연산을 지시한다.

```c
and dst, src: dst와 src의 비트가 모두 1이면 1, 아니면 0

[Register]
eax = 0xffff0000
ebx = 0xcafebabe
[Code]
and eax, ebx
[Result]
eax = 0xcafe0000
```

```c
or dst, src: dst와 src의 비트 중 하나라도 1이면 1, 아니면 0

[Register]
eax = 0xffff0000
ebx = 0xcafebabe
[Code]
or eax, ebx
[Result]
eax = 0xffffbabe
```

---

## 논리 연산 xor & not

```c
xor dst, src: dst와 src의 비트가 서로 다르면 1, 같으면 0

[Register]
eax = 0xffffffff
ebx = 0xcafebabe
[Code]
xor eax, ebx
[Result]
eax = 0x35014541
```

```c
not op: op의 비트 전부 반전

[Register]
eax = 0xffffffff
[Code]
not eax
[Result]
eax = 0x00000000
```

---

## 비교

두 피연산자의 값을 비교하고, 플래그를 설정한다.

```c
cmp op1, op2: op1과 op2를 비교

cmp는 두 피연산자를 빼서 대소를 비교합니다. 연산의 결과는 op1에 대입하지 않습니다.

예를 들어, 서로 같은 두 수를 빼면 결과가 0이 되어 ZF플래그가 설정되는데, 이후에 CPU는 이 플래그를 보고 두 값이 같았는지 판단할 수 있습니다.

[Code]
1: mov rax, 0xA
2: mov rbx, 0xA
3: cmp rax, rbx ; ZF=1
```

```c
test op1, op2: op1과 op2를 비교

test는 두 피연산자에 AND 비트연산을 취합니다. 연산의 결과는 op1에 대입하지 않습니다.

예를 들어, 아래 코드에서 처럼 0이된 rax를 op1과 op2로 삼아 test를 수행하면, 결과가 0이므로 ZF플래그가 설정됩니다. 이후에 CPU는 이 플래그를 보고 rax가 0이었는지 판단할 수 있습니다.

[Code]
1: xor rax, rax
2: test rax, rax ; ZF=1
```

---

## 분기

rip를 이동시켜 실행 흐름을 바꾼다.

```c
jmp addr: addr로 rip를 이동시킵니다.

[Code]
1: xor rax, rax
2: jmp 1 ; jump to 1
```

```c
je addr: 직전에 비교한 두 피연산자가 같으면 점프 (jump if equal)

[Code]
1: mov rax, 0xcafebabe
2: mov rbx, 0xcafebabe
3: cmp rax, rbx ; rax == rbx
4: je 1 ; jump to 1
```

```c
jg addr: 직전에 비교한 두 연산자 중 전자가 더 크면 점프 (jump if greater)

[Code]
1: mov rax, 0x31337
2: mov rbx, 0x13337
3: cmp rax, rbx ; rax > rbx
4: jg 1  ; jump to 1
```

---

## Stack pop, push

##### POP

```c
문제
[Register]
rsp = 0x7fffffffc400
[Stack]
0x7fffffffc400 | 0x0  <= rsp
0x7fffffffc408 | 0x0
[Code]
push 0x31337
```

```c
결과
[Register]
rsp = 0x7fffffffc3f8
[Stack]
0x7fffffffc3f8 | 0x31337  <= rsp
0x7fffffffc400 | 0x0
0x7fffffffc408 | 0x0
```

##### PUSH

```c
문제
[Register]
rax = 0
rsp = 0x7fffffffc3f8
[Stack]
0x7fffffffc3f8 | 0x31337 <= rsp 
0x7fffffffc400 | 0x0
0x7fffffffc408 | 0x0
[Code]
pop rax
```

```c
결과
[Register]
rax = 0x31337
rsp = 0x7fffffffc400
[Stack]
0x7fffffffc400 | 0x0 <= rsp 
0x7fffffffc408 | 0x0
```

---

## 프로시저(Procedure)

특정 기능을 수행하는 코드 조각이다.

반복되는 연산을 프로시저 호출로 대체할 수 있어서 전체 코드의 크기를 줄일 수 있고 기능별로 코드 조각에 이름을 붙일 수 있게 되어 코드의 가독성을 크게 높일 수 있다.

##### Call

연산

push return_address
jmp addr

```c
문제
[Register]
rip = 0x400000
rsp = 0x7fffffffc400 
[Stack]
0x7fffffffc3f8 | 0x0
0x7fffffffc400 | 0x0 <= rsp
[Code]
0x400000 | call 0x401000  <= rip
0x400005 | mov esi, eax
...
0x401000 | push rbp
```

```c
결과
[Register]
rip = 0x401000
rsp = 0x7fffffffc3f8
[Stack]
0x7fffffffc3f8 | 0x400005  <= rsp
0x7fffffffc400 | 0x0
[Code]
0x400000 | call 0x401000
0x400005 | mov esi, eax
...
0x401000 | push rbp  <= rip
```

---

##### leave

연산

mov rsp, rbp

pop rbp

```c
문제
rsp = 0x7fffffffc400
rbp = 0x7fffffffc480
[Stack]
0x7fffffffc400 | 0x0 <= rsp
...
0x7fffffffc480 | 0x7fffffffc500 <= rbp
0x7fffffffc488 | 0x31337 
[Code]
leave
```

```c
결과
[Register]
rsp = 0x7fffffffc488
rbp = 0x7fffffffc500
[Stack]
0x7fffffffc400 | 0x0
...
0x7fffffffc480 | 0x7fffffffc500
0x7fffffffc488 | 0x31337 <= rsp
...
0x7fffffffc500 | 0x7fffffffc550 <= rbp
```

##### POP

연산

pop rip

```c
문제
[Register]
rip = 0x401000
rsp = 0x7fffffffc3f8
[Stack]
0x7fffffffc3f8 | 0x400005    <= rsp
[Code]
0x400000 | call 0x401000
0x400005 | mov esi, eax
...
0x401000 | mov rbp, rsp  
...
0x401007 | leave
0x401008 | ret <= rip
```

```c
결과
[Register]
rip = 0x400005
rsp = 0x7fffffffc3f8
[Stack]
0x7fffffffc3f8 | 0x400005
0x7fffffffc400 | 0x0    <= rsp
[Code]
0x400000 | call 0x401000
0x400005 | mov esi, eax   <= rip
...
0x401000 | mov rbp, rsp  
...
0x401007 | leave
0x401008 | ret
```

---

## 스택프레임(StackFrame)

함수별로 서로가 사용하는 스택의 영역을 구분하기 위해 스택 프레임을 사용한다.

A라는 함수가 B라는 함수를 호출하는데 둘이 같은 스택 영역을 사용하면 B에서 A의 지역 변수를 모두 오염시킬 수 있다. 이후 B에서 반환 뒤 A는 정상적인 연산이 불가능 하므로 임시값들을 저장한다.

- call addr : addr의 프로시저를 호출한다.
- leave : 스택 프레임을 정리한다.
- ret : 호출자의 실행 흐름으로 돌아간다.

---
