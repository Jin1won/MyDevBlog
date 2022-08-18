# Prototype

2022/08/18

## 1. Prototype?

- JavaScript의 모든 객체는 Prototype이라는 객체를 가지고 있다. 모든 객체는 그들의 `[[Prototype]]`라는 **숨김 property**를 갖는다. 이 숨김 property 값은 null 이거나 다른 객체에 대한 참조가되는데, 다른 객체를 참조하는 경우 참조 대상을 **Prototype**이라 부릅니다.

  ![image](https://user-images.githubusercontent.com/76507701/185411051-42e2c801-0d76-4589-bbf4-08c3de58bac7.png)

## 2. Prototype 상속과정

- 객체에서 **property를 읽으려 할 때** 해당 **property가 없으면** JavaScript는 **자동으로 Prototype에서 property를 찾는다.** 이러한 동작 방식을 **Prototype 상속**이라 부른다.

- `[[Prototype]]` property는 내부에 있는 숨김 property이지만 `[[Prototype]]` 의 getter·setter인 `__proto__`를 사용하면 Prototype값을 설정할 수 있다.

다음과 같은 예시가 있다고 하자.

```
let animal = {
eats: true
};
let rabbit = {
jumps: true
};

rabbit.__proto__ = animal;

alert( rabbit.eats );
alert( rabbit.jumps );
```

- animal 이 rabbit 의 프로토타입이 되도록 설정했다.
- alert 함수가 rabbit.eats property를 읽으려 했는데, rabbit엔 eats 라는 property가 없다. 이때 JavaScript는 `[[Prototype]]` 이 참조하고 있는 객체인 animal에서 eats를 얻어낸다.

![image](https://user-images.githubusercontent.com/76507701/185425973-e322242a-6807-4c49-a36c-74e3826ba2d9.png)

- method또한 property와 같이 상속받아서 사용할 수 있다. 다음 예시를 보자.

```
let animal = {
eats: true,
walk() {
alert("동물이 걷습니다.");
}
};
let rabbit = {
jumps: true,
__proto__: animal
};

rabbit.walk(); // 동물이 걷습니다.
```

- Prototype인 animal에서 walk 를 자동으로 상속받았기 때문에 rabbit 에서도
  walk 를 호출할 수 있다.

![image](https://user-images.githubusercontent.com/76507701/185426863-1d88880b-4b57-4d98-a967-e60e5e3e9ed9.png)
