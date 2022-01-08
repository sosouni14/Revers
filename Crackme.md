## Crackme
abexcm1.exe 파일을 이용하여 리버싱 분석을 한다.

> 어셈블리로 작성하여 EP 코드가 짧다.

---

###### C언어 코드 복구

```c
#include<stdio.h>
#include<Windows.h>
int main()
{ 
    int eax = 0;
    int esi = 0; 
    MessageBoxA(NULL, "Make me think your HD is a CD-Rom", "abex' 1st crackme", MB_OK);
    
    eax = GetDriveTypeA("C:\\");
    esi++; 
    eax--; 
    esi++; 
    esi++; 
    eax--; 
    
    if (eax == esi) 
    { 
        MessageBoxA(NULL, "Ok, I really think that your HD is a CD-ROM! :p", "YEAH!", MB_OK); 
    } 
    else 
    { 
        MessageBoxA(NULL, "Nah... This is not a CD-ROM Drive!", "Error", MB_OK); 
    } 
    return 0; 
}
```



###### Main Srceen

![crackmain](https://i.postimg.cc/7L9J4X9r/crackmain.png)

처음 보이는 파일 화면이다.

MessageBoxA 가 총 3개 있는게 보인다. EP에 있는 메시지는 프로그램을 실행 시키면 뜨는 창이다.

이번 프로그램 과정은

프로그램의 흐름을 분석 한 뒤 문자열로 이벤트 시점을 찾는다.

이벤트의 성공하였을 경우를 찾아 흐름을 변경해줄 것이다.

---

###### Main Registers



![crackre](https://i.postimg.cc/wMrsPVzc/crackre.png)

ESI가 401000 으로 EP 이다.  첫번째 MessageBoxA의 안에서 ESI = FFFFFFFF 로 세팅되어 있다.

---

###### GetDriveType

![crack3](https://i.postimg.cc/KvR8BQmQ/crack3.png)

GetDriveType 의 함수가 끝나면서 EAX 값이 3이 된 것을 볼 수 있다.

EAX 값이 3인 이유는 드라이브의 고정된 미디어가 있으므로 함수의 반환 값이 3이 되었다.(RootPathName = "c:\\")

> GetDriveType 반환 값

| **GetDriveType**        |                          **반환값**                          |
| ----------------------- | :----------------------------------------------------------: |
| **DRIVE_UNKNOWN 0**     |             드라이브 유형을 확인할 수 없습니다.              |
| **DRIVE_NO_ROOT_DIR 1** | 루트 경로가 잘못되었습니다. 예를 들어 지정된 경로에 마운트된 볼륨이 없습니다. |
| **DRIVE_REMOVABLE 2**   | 드라이브에 이동식 미디어가 있습니다. 예를 들어, 플로피 드라이브, 썸 드라이브 또는 플래시 카드 판독기입니다. |
| **DRIVE_FIXED 3**       | 드라이브에 고정 미디어가 있습니다. 예를 들어, 하드 디스크 드라이브 또는 플래시 드라이브. |
| **DRIVE_REMOTE 4**      |          드라이브가 원격(네트워크) 드라이브입니다.           |
| **DRIVE_CDROM 5**       |              드라이브는 CD-ROM 드라이브입니다.               |
| **DRIVE_RAMDISK 6**     |                 드라이브는 RAM 디스크입니다.                 |



---

###### INC & DEC

![crack5](https://i.postimg.cc/1XTPvhyB/crack5.png)

INC(Increase) 값을 1증가, DEC(Decrease) 값을 1 감소 시킨다.

CMP 로 비교문 전까지 ESI = 3 이며 EAX는 1인 것을 계산할 수 있다. 

---

###### EAX 와 ESI 값

![crack4](https://i.postimg.cc/RFRjDrLf/crack4.png)

INC 와 DEC 를 만나면서 EAX 와 ESI 값이 달리지는 것을 확인 할 수 있다.

---

###### JE

![JE](https://i.postimg.cc/zGP5mcHg/crack6.png)

CMP 에서 EAX 와 ESI 가 같을 경우를 비교하고

같을 경우 JE 인 조건분기에서 40103D 또는 401028 로 가게한다.

CMP 비교 두개의 값이 동일하면 SUB 결과는 0이고 ZF는 1이 된다. 하지만 지금의 상황은 ZF가 0이 나온다. 그러므로 JE는 40103D인 true 값으로 가지 못하고 false 값인 401028로 가게 된다.

이것을 true로 만들어서 40103D로 가게 만드는 법이 있다.



### True로 만들기(3가지)

##### 1. CMP

CMP 문을 JMP로 바꾸어서 40103D로 가게 만든다.

![jmp](https://i.postimg.cc/SxDyVKGk/jmp.png)

JMP문을 만나서 바로 40103D 로 들어간 것을 볼 수 있다. 그 이후 실행했을때

성공의 MessageBoxA가 나오는 것을 확인 할 수 있다.

![yeah](https://i.postimg.cc/B6V9khYv/yeah.png)



##### 2. NOP

CMP 에서 비교를 하는데 그 전에 EAX 값과 ESI 값을 공백으로 만든다.

![nop](https://i.postimg.cc/J7qwJzcq/nop.png)

이렇게 NOP 명령어를 넣어서 CMP 명령어에서 EAX, ESI 값이 같다는 것으로 알고

JE 명령어에서 40103D로 넘어갈 수 있게 만든다.



##### 3. EAX = 5

프로그램을 실행해서 CMP 명령어를 만나기전까지 EAX = 3 ESI = 1인 것을 알았다.

그러면 우리는 EAX 값을 의도적으로 5로 바꾸어서 CMP 명령어를 만나기 전까지 모든 명령어를 다 실행했을 경우 EAX와 ESI 값을 같게 만들어준다.

![EAX5](https://i.postimg.cc/7Pm7fjhR/EAX5.png)*

의도적으로 EAX 값을 5로 고쳐서 실행하게 만든다.

CMP 명령어를 만나기전까지 EAX 값과 ESI 가 같다는 것을 볼 수 있다.

![EAX=ESI](https://i.postimg.cc/fTdw60Gv/EAX-ESI.png)

EAX = ESI 이므로 CMP 명령어에서 ZF = 1 이므로 JE 명령어에서 true가 되어서 40103D 로 간다.

그리고 Yeah title인 MessageBoxA를 띄운다.







> 참고사이트
>
> [Crackme](https://blog.naver.com/hungjaksm/40201096800)
>
> [GetDriveType](https://docs.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-getdrivetypea)
>
> [c언어 코드 복구](https://ccurity.tistory.com/186)
