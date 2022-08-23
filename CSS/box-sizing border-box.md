# border-box & content-box

2022/08/23

## 1. box-sizing

- box-sizing 속성은 CSS의 테두리 영역의 크기를 결정한다. **content-box**와 **border-box** 속성값이 있다.

## 2. CSS Box Model

- 브라우저에서 각 태그의 영역은 Box Model에서 볼 수 있듯이 크게 4가지 영역으로 나뉜다.

![image](https://user-images.githubusercontent.com/76507701/185791575-75e2dc85-ac54-40ad-a50a-ccee2d4aef50.png)

- content : 글씨가 삽입되는 영역
- border : 테두리 영역
- padding : content와 border 사이
- margin : border와 다른 태그 영역 사이

## 3. content-box

- width 및 height를 content 영역에만 적용한다. border, padding, margin은 따로 계산되어 전체 영역이 설정한 값 보다 커질 수 있다.

정리하면,
태그 전체 크기 = content + border + padding + margin
content 크기 = content

다음과 같은 요소가 주어졌을 때, width와 height의 크기는 아래와 같다.

```
<div
  style="
    width: 100px;
    height: 100px;
    box-sizing: content-box;
    border: 10px black solid;
  "
>
  content
</div>
```

![image](https://user-images.githubusercontent.com/76507701/185793102-8cb1fc0c-390c-4521-b628-9e19911d3ff7.png)

정리하면,
태그 전체 width, height = 10px + 100px + 10px = 120px
content width, height = 100px

## 4. border-box

- width 및 height를 border, padding, margin을 모두 합산한 태그 전체 영역에 적용한다.

정리하면,
태그 전체 크기 = content + border + padding + margin
content 크기 = 태그 전체 크기 – border – padding – margin

```
<div
  style="
    width: 100px;
    height: 100px;
    box-sizing: border-box;
    border: 10px black solid;
  "
>
  content
</div>
```

![image](https://user-images.githubusercontent.com/76507701/185793238-74c548a9-9fa4-400e-b77c-d04661932d86.png)

정리하면,
태그 전체 width, height = 10px + 80px + 10px = 100px
content width, height = 100px – 10px – 10px = 80px

## 참고 사이트

https://dasima.xyz/css-box-sizing-content-box-vs-border-box/
