# PE Header

### Dos Header

Dos EXE Header 확장한 IMAGE_DOS_HEADER 구조체이다.

WORD e_magic : 실행 파일의 signature 이다.

WORD e_lfanew : 파일에 따라 가변적인 값을 갖으며 NT header의 옵셋를 표시를 한다. 그 기준이 되는 주소에 더해진 값을 더한 것이 NT Header 시작점 위치로 알 수 있다.

---

### Dos stub

존재 여부를 결정하는 옵션이기에 크기가 일정하지 않는다. 

Dos stub 가 없어도 파일 실행에 문제가 없다. 코드와 데이터 혼합으로 이루어져있다.

---

### NT Header

NT Header의 구조체 IMAGE_NT_HEADER는 32bit 와 64bit에서 사용하는 구조체가 다르다.

IMAGE_IPTIONAL_HEADER32 = 32bit , 구조체 크기는 248byte

IMAGE_IPTIONAL_HEADER64 = 694bit, 구조체 크기는 264byte

- Signature : 이것을 보고 PE 파일 구조인지 확인 할 수 있다.(50450000 으로 저장된다.)
- File Header : 파일의 실행 대상의 시스템,툴 이다.
- Optional Header : 구조체 멤버이다.



#### File Header

- Machine : 32bit 0x14, 64bit 0x8664 로 넘버의 값으로 CPU의 고유한 값을 알 수 있다.
- Number Of Section : 섹션의 개수를 나타내는데 정의된 섹션 중점으로 실제 섹션을 맞춘다. 0보다 커야되며 그렇지 않을 경우 에러가 발생된다.
- Size Of Optional Header : 이미 크기가 결정되어 있으므로  Size Of Optional Header 값을 보고 구조체의 크기를 인식한다.
- Characteristics : 파일의 정보를 나타내고 bit or 형식으로 이루어져있다. 파일 번호를 보고 어떤 형식으로 이루어져있는지 알 수 있다.

---

## IMAGE_FILE_HEADER

### Optional Header

PE 헤더 구조체 중에서 가장 크기가 큰 헤더이다.

- Magic : IMAGE_IPTIONAL_HEADER32 = 10B, IMAGE_IPTIONAL_HEADER64 = 20B 이다.

- AddressOfEntryPoint : 프로그램에서 최초로 실행되는 코드의 시작 주소이다.

- ImageBase 가상 메모리에서 PE 파일이 로딩, 맵핑되는 시작 주소를 나타낸다.

- SectionAlignment, FileAlignment : 메모리, 파일 에서의 최소단위를 나타낸 것이며, 섹션 크기는 반드시 각각 SectionAlignment, FileAlignment 의 배수가 되어야 한다. 

- SizeOfHeader : PE 헤더의 전체 크기를 나타낸다. FileAlignment의 배수여야 하며, SizeOfHeader 옵셋만큼 떨어진 위치에 첫 번째 섹션에 위치한다.

- Subsystem : 이 값을 보고 파일을 구분할 수 있다.(1. Driver 파일, 2. GUI 파일, 3. CUI 파일)

- NumberOfRvaAndSizes : 배열의 개수가 DataDirectory 를 나타내는데 기본 16개를 나타내는데 아닐 수도 있다.

- DataDirectory : 배열의 각 항목마다 정의된 값을 가진다.

  | Offset  | Description                                    |
  | ------- | ---------------------------------------------- |
  | 168/184 | Thread local storage : 쓰레드 지역 저장 초기화 |
  | 192/208 | Import address table : 함수 시작 위치          |

  > Thread : 쓰레드
  >
  > 하나의  프로세스내에서 여러 개의 실행 흐름을 두고 작업을 효율적으로 처리하기 위한 것이다. 여러 프로세스가 공유하는 하나의 스레드는 없지만 어떤 프로세스든 하나 이상의 스레드를 사용한다.
  >
  > 프로세스 내에서 stack만 따로 할당 받고 code, data, heap 영억은 공유한다.
  >
  > 스레드를 사용하는 이유는 메모리 절약, 오버헤드 절감, 통신비용을 절감하기 위해서이다.

---

### Section Header

 프로그램의 안정성을 위해 섹션마아다 권한이 다르다.

| 종류        | 용도                                                         |
| ----------- | ------------------------------------------------------------ |
| .text(code) | 프로그램의 자체라고 할수 있다. 실행코드를 가지고 있으며 코드, 실행, 읽기 속성으로 컴파일 후 .text에 저장이 된다. |
| .data       | data 섹션으로 초기화(비실행) , 읽기, 쓰기 속성으로 초기화된 전역 변수를 저장한다. |
| .rdata      | 초기화(비실행), 읽기 const 로 선언된 변수 같이 읽기만 가능한 데이터를 저장한다. |
| .bss        | 비초기화, 읽기, 쓰기 속성으로 초기화되지 않는 전역 변수를 저장한다. |
| .edata      | 초기화, 읽기 속성으로 EAT과 관련된 정보를 저장한다.          |
| .idata      | 초기화, 읽기 쓰기 속성으로 IAT 관련 정보를 저장한다.         |
| .rsrc       | 초기화, 읽기 속성으로 리소스를 저장한다.                     |

- VirtualSize : 메모리에서 섹션이 차지하는 크기이다.
- VirtualAddress 메모리에서 섹션의 시작 주소인 RVA 를 뜻한다. 이것은 SectionAlignment 에 맞게 결정되며 아무 값을 가질 수 없다.
- SizeOfRawData : 파일에서 섹션이 차지하는 크기이며, 빈공간이 생기는데 FileAlignment의 배수로 공간을 할당하기 때문이다.
- PointerToRawData : 파일에서 섹션의 시작 위치이다. 이것은 FileAlignment에 맞게 결정되며 아무 값을 가질 수 없다.
- Characteristics :  섹션의 속성 bit OR 조합으로 이루어진다.