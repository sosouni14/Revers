# HXD 구조 분석

![HxD1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD1.PNG)

HxD 실행 파일을 PEview 로 열었을 때 보이는 구조 형식이다.

DOS Header 부터 Section Header 까지 PE Header 이며, 그 밑으로는 Body에 들어가는 Section 들 이다.

---

## IMAGE_DOS_HEADER

![HxD2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD2.PNG)

IMAGE_DOS_HEADER 에서 꼭 봐야할 멤버가 있다.

- e_magic
- e_lfanew

이 두개가 중요하다. magic 은 DOS signature 을 알 수 있으며, lfanew 는 NT header의 **시작 위치의 옵셋**이 0x100으로 된 것을 보여준다.

---

## DOS Stub

![HxD3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD3.PNG)

PE 가 시작하기 전, NT Header 가 시작 전인 부분이 DOS Stub 부분이다. 코드와 데이터가 들어가져있으며 나머지 부분을 00, NULL 로 채웠다.

---

## NT_HEADER - Singature

![HxD4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD4.PNG)

PE Singature 인 50450000 을 볼 수 있다.  

---

## NT_HEADER - IMAGE_FILE_HEADER

![HxD5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD5.PNG)

- Machine : 실행 파일 **플랫폼**을 보여준다. 0x14C 는 Intel 386 이다.

- Number of Sections : 섹션의 **개수**를 보여준다. 0008 이므로 8개이다.

  

- Size of Optional Header : 다음에 이어지는 Optional Header **크기**를 알려준다. 그러므로 0xE0 이다.

![HxD6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD6.PNG)

114번째 부터 0xE0 크기 만큼(CODE 전까지)이 Optional Header 이다.



![HxD7](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD7.PNG)

- Characteristics : 파일의 속성을 bit OR 연산을 통해 값을 보여준다. **818F** 만큼 사용되었다.

  파일 일부분이 사라지거나 파일이 어떤 형태이거나 문제가 생겼을 때 이 속성부분을 보고 파일을 복구할 수 있다. **0002** 이므로 **exe** 파일 인 것을 알 수 있다.

---

## NT_HEADER - IMAGE_OPTIONAL_HEADER

![HxD8](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD8.PNG)

![HxD9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD9.PNG)

여기에서 중요하게 볼 것이 있다.

- Magic : 구조체를 보여준다. 0x10B 이므로 IMAGE_OPTIONAL_HEADER32 이다.
- Address of Entry Point : EP의 RVA 값을 가진다. 프로그램에서 최초 실행 코드의 시작 주소를 보여기에 **1633CC** 인것을 알 수 있다.
- Image Base : **PE 파일** 로딩되는 **시작 주소**이며 **exe** 파일의 값은 00400000 이다.
- Section Alignment : **메모리**에서 섹션의 최소 단위는 00001000 이다.
- File Aligment : **파일**에서 섹션의 최소 단위는 00000200 이다.
- Size of Image : **PE** 파일이 **메모리**에 로딩되었을 때 **가상 메모리**에서 PE Image가 차지하는 크기를 보여준다.  크기는 001BB000 이다.
- Size of Headers : PE 헤더의 전체 크기를 보여준다. File Alignment 의 배수여야 한다. 그러므로 400 이다.
- Subsystem : 실행 파일을 구분한다. 0002 이므로 GUI 파일이다.
- Number of Data Directories : 총 10(16진수) 의 구조체의 배열 를 갖는다.
- DataDirectory : 개수를 세어보면 총 16개를 가진것을 알 수 있다.

---

# IMAGE_SECTION_HEADER CODE

![HxD10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/HxD10.PNG)

CODE 의 데이터가 43 4F 44 45 인 것을 볼 수 있다. 

- Virtual Size : **메모리**에서 섹션이 **차지**하는 크기는 0x1624A8 이다.

- RVA(Virtual Address) : **메모리**에서 섹션의 **시작 주소**는 0x10000 이다.

- Size of Raw Data : **파일**에서 섹션이 **차지**하는 크기는 0x162600 이다.

- Pointer to Raw Data :  **파일**에서 섹션의 **시작 위치**는  0x400 이다.

- Characteristics : 섹션의 속성을 보여준다.

  00000020			IMAGE_SCN_CNT_CODE

  20000000		    IMAGE_SCN_MEM_EXECUTE

  40000000		    IMAGE_SCN_MEM_READ

  60000020			Characteristics
