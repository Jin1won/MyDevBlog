# visibility hidden & display none

2022/08/16

## 1.CSS로 특정 영역을 숨기는 방법?

- 화면에 특정 영역을 보이지 않게 처리할 때, display:none과 visibility:hidden 속성을 사용한다. 두 속성 모두 화면에 있는 요소를 사라지게 하지만 차이점이 존재한다.

## 2. visibility:hidden과 display:none의 차이

### visibility:hidden

- 브라우저에서 요소를 숨기지만 **숨겨진 요소는 여전히 레이아웃에서 공간을 차지**한다.

```
<div>영역1</div>
<div style="visibility: hidden">visibility:hidden</div>
<div>영역2</div>
```

다음과 같은 코드에서 브라우저는 아래와 같이 요소를 출력한다.
![image](https://user-images.githubusercontent.com/76507701/185669811-3029eafd-44ad-44ac-b7ba-600d5558a489.png)

### display:none

- 문서 플로우에 요소를 남기는 visibility 속성과 달리, display : none은 **요소를 문서에서 완전히 제거**한다. HTML 코드는 존재하지만, **공간을 차지하지 않는다.**

```
<div>영역1</div>
<div style="display: none">display: none</div>
<div>영역2</div>
```

다음과 같은 코드에서 브라우저는 아래와 같이 요소를 출력한다.
![image](https://user-images.githubusercontent.com/76507701/185670057-03f010d5-e5db-4b4e-af7e-cf4474b261d3.png)
