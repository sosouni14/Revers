# PE File Format

PE 파일은 32bit 으로 구성되어 있으며  PE32 라고도 한다.

64bit 로 구성된 것은 PE+ 또는 PE32+ 라고 한다.

---

### PE File Format 종류

| 종류            | 주요 확장자        | 종류          | 주요 확장자 |
| --------------- | ------------------ | ------------- | ----------- |
| 실행 계열       | EXE, SCR           | 드라이브 계열 | SYS, VXD    |
| 라이브러리 계열 | DLL, OCX, CPL, DRV | 오브젝트 계열 | OBJ         |

OBJ 파일을 PE 파일이라고 하지만 OBJ 파일 자체로 어떠한 형태의 실행도 불가능하므로 리버싱에서 관심 가질 필요가 거의 없다.

---

### 기본 구조

- DOS header ~ Section header 까지 PE header

- 그 밑으로 Section들을 합쳐서  PE body

파일이 메모리에 로딩되면 모양이 달라진다.(Section의 크기, 위치 등)

보통 **코드(.text), 데이터(.data), 리소스(.rsrc)** 섹션으로 나뉘어서 저장한다.

개발 도구와 빌드 옵션에 따라서 섹션의 이름, 크기, 개수, 저장 내용 등이 달라진다.

하지만 **용도별로 여러 섹션이 나뉘어서 저장**된다.

섹션 헤더에 각 Section에 대한 파일/ 메모리에서의 크기, 위치, 속성들이 정의 되어 있다.

PE 헤더의 끝부분과 각 섹션의 끝에는 **NULL Padding**이라고 불리는 영역이 존재한다.

최소 단위 개념을 사용하는데 PE 파일에도 같은 개념으로 적용되었다.

파일/ 메모리에서 섹션의 시작 위치는 각 파일/ 메모리의 최소 기본 단위의 배수에 해당하는 위치, 빈 공간은 NULL로 채운다.

NULL로 값을 채우는 이유는 **최소 기본 단위**를 사용하기 위해서이다. 다음 섹션의 위치를 빨리 찾을 수 있다.

---

### VA & RVA

**VA**(Virtual Address)는 프로세스 가상 메모리의 **절대주소**이다.

**RVA**(Rela-tive Virtual Address)는 어느 **기준 위치**(ImageBase)에서부터의 **상대주소**를 뜻한다.

**RAV + ImageBase = VA**



PE 헤더 내의 정보가 RVA 형태로 된 것이 많은 이유는?

PE 파일(주로 DLL)이 특정 위치에 로딩되는 순간 다른 PE파일(DLL)이 있을 수 있다.

그러므로 비어있는 위치에 로딩되고 정보를 RVA로 해두면 재배치 발생이 일어나도 기준 위치에 대한 상대  주소는 **변하지 않아** 원하는 정보에 엑세스할 수 있다. 

---

32bit Windows OS에서 각 프로세스에게 4GB 크기의 가상 메모리가 할당된다. 따라서 프로세스에서 VA 값의 범위는 00000000 ~ FFFFFFFF 까지이다.

----

## PE 헤더

### DOS Header

PE 만들때 많이 사용한 DOS 파일의 하위 호환성을 고려해서 만들었다.

PE 헤더의 제일 앞 부분에는 기존 DOS EXE Header를 확장시킨 IMAGE_DOS_HEADER 구조체가 존재한다.

IMAGE_DOS_HEADER 구조체는 크기 40(16진수) 64Byte 이다.

- e_magic : DOS Signature(4D5A → ASCII 값 'MZ')
- e_lfanew : NT header의 옵셋을 표시(파일에 따라 가변적인 값을 가진다.)

PE 파일은 시작부분(e_magic) 에 DOS Signature("MZ") 가 존재하고

e_lfanew 값이 가리키는 위치에 NT Header 구조체가 존재한다.

---

### DOS Stub

DOS Stub 은 요즘 대부분 있지만 크기는 일정하지 않으며 이 부분은 없어도 파일 실행에 문제 없다.

DOS Stub 은 **코드**와 **데이터**의 혼합으로 이루어져있다.

DOS Stub 부분에서 문자가 적혀있다면 명령어 부분에서도 문자열을 확인 할 수 있다.

---

### NT Header

NT header 구조체 IMAGE_NT_HEADERS 구조체의 크기는 F8(248 Byte) 이다.

IMAGE_NT_HEADERS 구조체는 3개의 멤버로 되어있다.

- Signature 로 50450000("PE"00) 값
- File Header
- Optional Header

---

### NT Header - File Header

IMAGE_FILE_HEADER 구조체이다.

- Machine
- NumberOfSections
- SizeOfOptionalHeader
- Characteristics

4가지의 멤버가 정확히 세팅되지 않으면 실행되지 않는다.



#### Machine

cpu별로 고유한 값이며 32bit Intel x86호환 칩은 14c값을 가진다.



#### NumberOfSections

PE 파일은 코드, 데이터, 리소스 등이 각각의 섹션에 나뉘어 저장된다.

그 **섹션의 개수**를 나타내는 것이 **NumberOfSections** 이다. 



#### SizeOfOptionalHeader

IMAGE_OPTIONAL_HEADER32 구조체 크기를 나타내는 SizeOfOptionalHeader 이다.

Windows의 PE로더는 IMAGE_FILE_HEADER의 SizeOfOptionalHeader 값을 보고 IMAGE_OPTIONAL_HEADER32 구조체의 크기를 인식한다.



#### 