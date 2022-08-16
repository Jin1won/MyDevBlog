# Debounce & Throttle

2022/08/16

## 1. 중복적인 이벤트?

- 개발을 하다 보면 여러가지 이벤트가 연이어서 발생하는 경우가 종종 생긴다. 예를 들어, 스크롤 이벤트의 경우 스크롤링 할 때마다 발생하는데, 그 때마다 같은 작업을 실행하게 되면 브라우저 성능 문제가 발생할 수 있다.

- Debounce와 Throttle는 이런 문제를 해결하기 위한 방법이다.

## 2. Debounce? Throttle?

- Debounce는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정시간이 경과한 후에 이벤트를 한번만 호출되도록 한다. 즉, 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한번만 이벤트 핸들러가 호출되도록 한다.

- Throttle는 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트가 최대 한번만 호출되도록 한다. 즉, 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호츨 주기를 만든다.

## 3. 스크롤링 이벤트를 통한 예시

- 스크롤 이벤트가 발생할 때 마다 count를 올리고 로그를 찍는 상황에서 화면은 다음과 같이 출력된다.
  ![image](https://blog.kakaocdn.net/dn/kDNTJ/btq6DRvBwph/3k2oX7mlFcAukK21dBgtk1/img.gif)

- Debounce를 적용한다면 다음과 같이 화면이 출력된다.
  ![image](https://blog.kakaocdn.net/dn/8bbG6/btq6DDjSpjY/CrB42nQ09EVeMfDUNZiOi1/img.gif)
- 마지막으로 발생한 이벤트에 대해서만 이벤트 콜백 함수가 호출된다.

- Throttle를 적용한다면 다음과 같이 화면이 출력된다.
  ![image](https://blog.kakaocdn.net/dn/swG1i/btq6zzX0KdQ/SKg5N7hqR6KMTSOGRVgRek/img.gif)
- Throttler가 이벤트를 조이고 있는 경우 즉, 설정한 시간 동안 발생한 이벤트는 최대 1번만 발생한다.

## 참고 사이트

https://programming119.tistory.com/241
