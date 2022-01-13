## API

#### LoadStringW

```c++
int LoadStringW(
  [in, optional] HINSTANCE hInstance,
  [in]           UINT      uID,
  [out]          LPWSTR    lpBuffer,
  [in]           int       cchBufferMax
);
```

`HINSTANCE hInstance` : 문자열 리소스를 포함한 모듈의 인스턴스 핸들이다.

`UINT      uID` : 인수로 읽어올 문자열의 ID 이다.

`LPWSTR    lpBuffer` : 문자열을 읽을 버퍼이다.

`int       cchBufferMax` : 버퍼의 크기(문자) 이다.

문자열 리소스가 없으면 0이다.

---

`LoadStringW(hInstance, 0x67u, &WindowName, 100);`

 이 함수를 해석해 보자면 `hInstance` 인스턴스에 `0x67u` 로 입력된 인수로 문자열 `100` 길이를`&WindowName` 버퍼로 읽어 올 것이다. 

라고 해석 할 수 있을 것 같다.

---

#### LoadIconW

```c++
HICON LoadIconW(
  [in, optional] HINSTANCE hInstance,
  [in]           LPCWSTR   lpIconName
);
```

`HINSTANCE hInstance` : 문자열 리소스를 포함한 모듈의 인스턴스 핸들이다.

`LPCWSTR   lpIconName` : 읽을 아이콘 리소스의 이름이다.

---

`LoadIconW(hInstance, (LPCWSTR)0x6B);`

이 함수를 해석해 보자면 `hInstance` 인스턴스에 `(LPCWSTR)0x6B` 을 읽어오겠다.

라고 해석 할 수 있을 것 같다.

---

#### LoadCursorW

```c++
HCURSOR LoadCursorW(
  [in, optional] HINSTANCE hInstance,
  [in]           LPCWSTR   lpCursorName
);
```

`HINSTANCE hInstance` : 실행 파일에서 읽을 커서가 포함된 모듈의 인스턴에 대한 핸들이다. 

`LPCWSTR   lpCursorName` : 읽을 커서 리소스의 이름이다.

---

`LoadCursorW(0i64, (LPCWSTR)0x7F00);`

이 함수를 해석해 보자면 `0i64` 인스턴스에 `(LPCWSTR)0x7F00)` 커서를 읽어오겠다.

라고 해석 할 수 있을 것 같다.

---

#### RegisterClassExW

```c++
ATOM RegisterClassExW(
  [in] const WNDCLASSEXW *unnamedParam1
);
```

`const WNDCLASSEXW` : `WNDCLASSEXW` 구조에 대한 포인터이다. 함수에 전달하기 전에 적절한 클래스 속성으로 구조를 채운다.

`WNDCLASSEXW` : 창 클래스 정보를 포함한다. `RegisterClassExW` 와 함께 쓰이는 구조체이다.

이 함수는 실해하면 반환 값이 0이다.

---

#### CreateWindowExW

```C++
HWND CreateWindowExW(
  [in]           DWORD     dwExStyle,
  [in, optional] LPCWSTR   lpClassName,
  [in, optional] LPCWSTR   lpWindowName,
  [in]           DWORD     dwStyle,
  [in]           int       X,
  [in]           int       Y,
  [in]           int       nWidth,
  [in]           int       nHeight,
  [in, optional] HWND      hWndParent,
  [in, optional] HMENU     hMenu,
  [in, optional] HINSTANCE hInstance,
  [in, optional] LPVOID    lpParam
);
```

`DWORD     dwExStyle` : 생성 중인 창의 확장 윈도우 스타일이다.

`LPCWSTR   lpClassName` : 윈도우 클래스 이름이다.

`LPCWSTR   lpWindowName` : 아이콘의 이름이나 ID를 지정하는 것이다.

`DWORD     dwStyle` : 윈도우 스타일이다.

`int       X` : 창의 X 좌표

`int       Y` : 창의 Y 좌표

`int       nWidth` : 창의 높이 X

`int       nHeight` : 창의 넓이 Y

`HWND      hWndParent` : 창의 부모 또는 소유자 창에 대한 핸들이다.

`HMENU     hMenu` : 메뉴에 대한 핸들 또는 자식 윈도우 ID 이다.

`HINSTANCE hInstance` : 창과 연결할 모듈의 인스턴스 핸들이다.

`LPVOID    lpParam` : 작성 데이터를 담고 있다.

---

#### GdiplusStartup

```C++
Status GdiplusStartup(
  ULONG_PTR                 *token,
  const GdiplusStartupInput *input,
  GdiplusStartupOutput      *output
);
```

`ULONG_PTR` : 토큰(token)을 받는 `ULONG_PTR` 에 대한 포인터이다.

GDI+ 사용을 마치면 토큰을 `GdiplusShutdown`에 전달한다.

`GdiplusShutdown` : GdiplusStartup 에 대한 이전 호출에서 반환된 토큰이다.

`const GdiplusStartupInput` : const GdiplusStartupInput 구조체에 주소가 들어간다.디버그 콜백 함수, 백그라운드 스레드 허용 여부등을 지정한다.

`GdiplusStartupOutput` :  구조체의 주소가 들어가면 알림 후크 기능에 대한 포인터가 호기 값으로 반환된다.

---

#### ShowWindow

```C++
BOOL ShowWindow(
  [in] HWND hWnd,
  [in] int  nCmdShow
);
```

`HWND hWnd` : 창에 대한 핸들이다.

`int  nCmdShow` : 창이 표시되는 방식을 제어한다. 윈도우 및 컨트롤의 표시, 숨김, 최대화, 최소화 등에 대한 방식의 제어 값이 입력된다.

---

#### UpdateWindow

```C++
BOOL UpdateWindow(
  [in] HWND hWnd
);
```

`HWND hWnd` : 업데이트 할 창에 대한 핸들이다.

---

#### LoadAcceleratorsW

```C++
HACCEL LoadAcceleratorsW(
  [in, optional] HINSTANCE hInstance,
  [in]           LPCWSTR   lpTableName
);
```

`HINSTANCE hInstance` : 실행 파일에 읽어야할 가속기 테이블이 포함된 모듈에 대한 핸들이다.

`LPCWSTR   lpTableName` : 읽어야할 가속기 테이블의 이름이다. 

가속기 테이블 리소스의 리소스 ID를 지정하고 상위 단어에서 0을 지정할 수 있다. 결국 매크로 명령만으로 효과적인 작업을 만들 수 있게 하는 함수이다.

---

#### GetMessageW

```C++
BOOL GetMessageW(
  [out]          LPMSG lpMsg,
  [in, optional] HWND  hWnd,
  [in]           UINT  wMsgFilterMin,
  [in]           UINT  wMsgFilterMax
);
```

`LPMSG lpMsg` : 입력 받은 메시지들을 모아두었다가 합쳐진 정보이다. MSG 구조체에 대한 포인터이다.

`HWND  hWnd` : 메시지를 검색할 창에 대한 핸들이다.

`UINT  wMsgFilterMin` : 검색할 메시지가 가장 낮은 정수 값이다. 

`UINT  wMsgFilterMax` : 검색할 메시지가 가장 높은 정수 값이다.

---

#### BeginPaint

```C++
HDC BeginPaint(
  [in]  HWND          hWnd,
  [out] LPPAINTSTRUCT lpPaint
);
```

`HWND          hWnd` : 다시 칠할 창에 대한 핸들이다.

`LPPAINTSTRUCT lpPaint` : 페인팅 정보를 수신할 `PAINTSTRUCT` 구조에 대한 포인터이다.

**WM_PAINT** 은 `Beginpaint` 를 사용한다.

`Beginpaint` 는 지워지지 않도록 `EndPoint` 함수를 불러오기 때문에 캐럿을 화면에 복원하고 영역을 해제한다.

`PAINTSTRUCT` : 페인팅, 그림에 사용되는 구조체이다.