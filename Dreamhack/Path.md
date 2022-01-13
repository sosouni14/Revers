## Path(Dreamhack)

![path4](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path4.PNG)

프로그램을 실행했을 때 보이는 이미지 창이다.

---

![path2](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path2.PNG)

![path3](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path3.PNG)

path 파일을 열었을 때 처음으로 보이는 코드 화면이다.

처음에는 정적 분석으로 한다.

`Call`  부분에 함수들을 불러오는 것을 확인 할 수 있다.

`LoadStringW, LoadIconW, LoadCursorW, RegisterClassExW, CreateWindowExW` 함수들이 보인다.

`LoadStringW` : 버퍼에 있는 문자열을 복사하거나 읽어서 반환한다.

`LoadIconW` : Icon

`LoadCursorW` : 지정된 커서 리소스를 로드한다.

`RegisterClassExW` : 클래스 속성으로 구조를 채운다. 반환 값으로 등록된 클래스를 식별하는 원자는 `CreateWindowEx` 가 있다.

`CreateWindowExW` : 확장된 창 스타일로 겹친, 팝업 또는 자식 창을 만든다.

그러면 `RegisterClassExW` 에서 선언하고 반환 할 때 `CreateWindowExW`으로 가는 것을 예상할 수 있다.

---

### RegisterClassExW

![path5](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path5.PNG)

함수를 디컴파일하여 보았을 때 `v11` 을 들고 있는 것을 확인 할 수 있다.

`v11.lpfnWndProc = (WNDPROC)sub_7FF6FF7932F0;` 으로 `v11` 에 값이 들어가 있으며 무슨 값이 들어갔는지 확인하기 위해 이 부분도 디컴파일을 한다.

---

### sub_7FF6FF7932F0

![path6](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path6.PNG)

Switch 문으로 이루어진 것을 확인할 수 있다.

여기서 함수를 제외한 값들을 하나씩 디컴파일 하여 들어가서 무엇을 들고 있는지 확인해 본다.

`sub_7FF6FF792C40();` 에 들어가니 값들이 선언 되고 있는 것을 확인할 수 있다.

---

### sub_7FF6FF792C40

![path7](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path7.PNG)

![path8](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path8.PNG)

첫번째 부분에 BP를 걸고 실행했을 때 어떻게 되는지 확인한다.

![path9](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path9.PNG)

먼저 가려지는 부분이 실행되는 것을 알 수 있게 되었다.

![path10](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path10.PNG)

![path11](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path11.PNG)

이 구간을 더 실행 했을 시 글자가 나오는 것을 확인 할 수 있으며 먼저 그림을 그려지는 부분을 없애면 적혀진 글자를 알 수 있다는 것을 알아내었다.

---

### sub_7FF6FF792B80

파일을 디버깅하지 않고 G 단축키로 원하는 주소지로 점프한다. 그림이 그려지는 곳을 알아냈기에

`sub_7FF6FF792B80` 적어서 그곳으로 점프한다.

![path12](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path12.PNG)

`mov [rsp+arg_8], rbx`  부분이 시작 부분인 것을 알아내었다.

이 부분을 실행되지 않게  `retn` 명령어를 넣는다.

> 원하는 주소부분에 클릭을 한 뒤 Edit → Path Program → Assemble

명령어를 바꾼 뒤 패치한 부분을 적용한다.

>  Edit → Path Program → Apply patches to input file

이 부분 명령어를 바꾼 뒤 실행 버튼을 하였을 때 값이 보이는 창이 뜬다.

![path1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/path1.PNG)

글씨들을 가리는 막대기가 나오지 않고 글씨만 나오는 것을 확인 할 수 있다.



이것과 다르게 다른 방법으로 글씨가 생성되는 부분에 값들을 찾아서 직접 프로그램을 만들어 실행을 하는 방법도 있다.