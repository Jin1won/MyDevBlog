# JavaScript 비동기 처리 과정

2022/08/25

## JavaScript는 어떤 언어인가?

- 프로토타입 객체 기반의 **싱글 쓰레드 스크립트 프로그래밍 언어**이다. 싱글 쓰레드로 동작하기 때문에 여러 동작을 처리해야 하는 경우 **WebAPI에게 비동기 동작을 위임**함으로써 다른 동작을 실행할 수 있다. WebAPI를 통해 무거운 작업을 분리된 스레드에서 처리하면 주 쓰레드가 멈추거나 느려지지 않고 동작할 수 있다.

## JavaScript 코드 실행 시 구성요소

- **JavaScript 엔진** : **Heap, Call Stack**을 포함하는 JavaScript 해석 엔진
  - **Heap** : 참조타입(객체 등) 데이터가 저장된다.
  - **Call Stack** : 원시타입(숫자 등) 데이터가 저장된다.(실행컨텍스트를 통해 변수 식별자 저장, 스코프 체인 및 this 관리, 코드 실행 순서 관리 등을 수행)
- **WebAPI** : DOM 조작, setTimeout, 네트워크 요청/응답(AJAX) 등의 브라우저 고유 기능을 수행한다.
- **Callback Queue(Task Queue)** : WebAPI 로부터 전달받은 콜백 함수를 저장한다.
  - **Microtask Queue** : Promise Callback같은 함수들이 저장된다
  - **Macrotask Queue** : setTimeout같은 함수들이 저장된다.

## 이벤트 루프

- 이벤트 루프란 **Callback Queue에서 Callback 함수를 하나씩 꺼내서 동작**시키는 루프를 말한다.

## 이벤트 루프 과정

다음과 같은 코드가 실행된다고 하자.

```
console.log("Hi!");

setTimeout(function timeout() {
    console.log("Click the button!");
}, 0);

console.log("Welcome to loupe.");
```

1. console.log("Hi!")가 **실행**되어 **Call Stack**으로 들어간다. 그 후, 콘솔을 **출력하고 해당 함수는 종료된다.**
   ![image](https://user-images.githubusercontent.com/76507701/185804591-f3fcbb74-a27e-4554-8c9d-e5c61a242bdd.png)

2. setTimeout **함수가 실행**되어 **Call Stack**으로 들어간다.
   ![image](https://user-images.githubusercontent.com/76507701/185804814-fd4aac0b-c1ff-4c4d-8677-a55ad6a410c9.png)

3. setTimeout 함수가 종료되고 timeout이라는 콜백함수가 **Web API에서 1초간 대기**한다.
   ![image](https://user-images.githubusercontent.com/76507701/185804849-cc4483b0-ca89-4fe3-a3cc-1cb2d05f795e.png)

4. timeout에 관계 없이 다음 함수인 console.log("Welcome to loupe.")가 **실행되어 Call Stack에 올라간다.**
   ![image](https://user-images.githubusercontent.com/76507701/185804856-1f8e0282-b8db-423c-9179-dcc72abdda4c.png)

5. **1초가 지나서** timeout 함수가 **Callback Queue**로 올라간다.
   ![image](https://user-images.githubusercontent.com/76507701/185804871-fec08f74-8082-4393-8b05-8015afea62e8.png)

6. console.log("Welcome to loupe.")가 실행이 끝나고, **Call Stack이 빈다.**
   ![image](https://user-images.githubusercontent.com/76507701/185804962-f87dda15-1fbf-4942-9c7a-51b3bb47d510.png)

7. Callback Queue에 있던 **timeout 함수를 이벤트 루프가 Call Stack으로 옮긴다.**
   ![image](https://user-images.githubusercontent.com/76507701/185804974-67e8e147-1e85-4d95-a0e3-f90a7dc4b9f5.png)

8. **timeout함수가 실행**되고, 함수 내부에 있던 **console.log("Click the button!")이 실행**되어 **Call Stack에 올라간다.**
   ![image](https://user-images.githubusercontent.com/76507701/185804980-83d6697d-1029-45cd-9740-bab292097801.png)

9. console.log("Click the button!")의 **실행이 끝**난 뒤, timeout **함수도 종료**된다.
   ![image](https://user-images.githubusercontent.com/76507701/185804992-ecd06260-282a-43b6-aa08-3dc1389958a3.png)

## 참고 사이트

https://www.youtube.com/watch?v=8aGhZQkoFbQ&feature=youtu.be
