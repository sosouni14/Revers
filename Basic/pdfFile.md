# PDF 파일 구조

![ba1](https://raw.githubusercontent.com/sosouni14/image_server/main/image_rev/ba1.PNG)

PDF 파일 구조는 총 4가지로 나뉘어져있다.

- Header : 8 byte로 PDF 시그니처와 문서의 버전 정보를 보여준다.
- Body : 실제 문서의 정보들을 포함하는 오브젝트와 이 객체들을 구성하는  트리형태로 서로 링크되어 있다.
- Cross-reference table(Xref Table) : 오브젝트(객체)를 참초할 때 사용되는 테이블이며, 객체들의 사용 여부와 식별 번호 등이 저장되어 있다.
- Trailer : File Body 에 존재하는 객체 중 최상위 객체가 어떤 객체인지, Xref Table 이 어디에 위치하는지 기록되어 있는 부분이다.

---

### PDF 파일 구조 분석

파일을 처음 열었을 때 PDF 와 obj 글자를 볼 수 있었다. 

파일 종류는 obj 파일 계열이라고 생각했으며 pdf 로 만들어졌다고 생각하였다.

header 부분에 해당된다고 생각하고 밑 부분을 보았을 때 데이터들이 존재하기에 stub 부분이라고 생각하였다. 그리고 PE 글자를 찾아볼려고 하였지만 보이지 않아 NT header 부분이 아직 창에 보이지 않는다고 생각되었다.

하지만 obj 파일은 pe 파일이지만 이 파일 자체로 실행 불가능한 파일이다. 그래서 obj 계열 종류인 pdf 파일의 구조는 다르다고 생각된다.

pdf 구조를 찾아보고 실행 파일을 열었을 때 보이는 부분을 보면서 알 수 있었던 것은

Header 와 Body 부분을 보았던 것을 알 수 있었다.

Header 부분에서 PDF 의 시그니처를 볼 수 있었으며,

Body 부분에서 100 obj 부분으로 오브젝트가 시작되는 거와 함께 endobj로 끝나면서 100 obj가 끝나는 것을 알 수 있었다.







> 참고
>
> [IMG](https://media.vlpt.us/images/tjddyd1592/post/d1dbcce4-48cf-479e-bf29-f936ea0c4d2f/image.png)