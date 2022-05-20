## PE 파일

PE 파일을 통해서 프로그램이 사용하는 API 또는 DLL 등 다양한 정보와 어느 메모리 주소에 로딩 되는지 확인할 수 있다.



#### PE 파일 종류

- 실행 : exe, SCR
- 라이브러리 : DLL, OCX, CPL, DRV
- 드라이버 : SYS, VXD
- 오브젝트 : OBJ



#### PE 구조

- 다양한 정보들이 PE Header에 구조체 형식 저장
- Dos Header ~ section Header 까지 PE Header
- section 들을 합쳐서 Body 에 해당
- 파일에서 **offset**, 메모리에서 **VA** 로 위치 표현
- PE 파일의 실행 영역은 .text



**offset** - 파일의 첫 바이트부터 거리를 뜻한다.

**VA** - 프로세스 가상 메모리의 **절대** 주소

**RVA** - 기준(**Image Base**) 로부터 **상대** 주소

**VA** = RVA + Image Base



> Null 로 값을 채우는 이유는 최소 기본 단위를 사용하기 위해서이다.

> RVA 형태가 많은 이유는 PE파일이 주로 DLL은 특정 위치에 로딩되는 순간 다른 PE파일인 DLL이 있을 수 있다. 그러므로 비어있는 위치에 로딩되고 정보를 **RVA** 로 해두면 **재배치** 발생이 일어나도 기준 위치에 대한 **상대주소** ***변하지 않아*** 원하는 정보에 **액세스** 할 수 있다.



##### IMAGE_DOS_HEADER(magic, ifanew)

- 5A4D signature : 4D 5A 리틀 엔디안 표기법



##### e_magic : 어떤 구조인지 알려주는 부분

| MZ = 4D5A             | ZIP = 5O4B          |
| --------------------- | ------------------- |
| **PDF = 25 50 44 46** | **PIVG = 89504E47** |



##### e-lfanew : Image_NT_Header 시작 주소

실제 이 파일이 어디서 시작하는지 알려주는 부분이며, 시작점을 알려주는 offset 역할



##### Dos stub

"This is program connot be run in Dos mode" 위도우에서 실행되는 파일임을 알려준다.

실행하지 않아도 정적분석을 통해 윈도우에서 실행되는지 DOS 에서 실행되는 것인지 알 수 있다.



##### OPTIONAL_HEADER

- magic
- address of entry point : 코드영역(.text) 크기
- sub system : 



##### - magic

- 실행하는 PE 파일의 구조체 구분
- 32bit(0x10B), 64bit(0x20B)



##### - size of code

- 코드영역(.text) 크기



##### - Address of Entry Point

- 프로그램 실제 시작주소



##### - Base of code

- 코드영역이 시작되는 상대주소(**RVA**)
- 코드영역 = **ImageBase + RVA**



##### -Image Base

- PE파일이 메모리에 로드되는 시작 주소
- exe(0x400000) , DLL(0x110000000)



##### - section Alignment

- 메모리에서 섹션의 최소 단위, 시작 수조는 이 값의 배수



##### - File Alignment

- 파일에서 섹션의 최소단위, 시작주소는 이 값의 배수



##### -size of Image

- PE파일이 메모리에 로딩될 때 전체 크기



##### -size of header

- 모든 헤더의 크기



##### -sub system

| 1(system driver) | 2(GUI) | 3(CUI) |
| :--------------- | ------ | ------ |



##### - Number of RVA and size

- Data Directory 의 구조체 멤버 개수



##### - SECTION_HEADER

- .text , .data , .rdata

