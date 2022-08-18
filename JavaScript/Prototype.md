# Prototype

2022/08/18

## 1. Prototype?

- JavaScript의 모든 객체는 Prototype이라는 객체를 가지고 있다. 모든 객체는 그들의 `[[Prototype]]`라는 **숨김 property**를 갖는다. 이 숨김 property 값은 null 이거나 다른 객체에 대한 참조가되는데, 다른 객체를 참조하는 경우 참조 대상을 **Prototype**이라 부른다.

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

alert( rabbit.eats ); //true
alert( rabbit.jumps ); //true
```

animal 이 rabbit 의 프로토타입이 되도록 설정했다.
alert 함수가 rabbit.eats property를 읽으려 했는데, rabbit엔 eats 라는 property가 없다. 이때 JavaScript는 `[[Prototype]]` 이 참조하고 있는 객체인 animal에서 eats를 얻는다.

![image](https://user-images.githubusercontent.com/76507701/185425973-e322242a-6807-4c49-a36c-74e3826ba2d9.png)

- method또한 property와 같이 상속받아서 사용할 수 있다. 다음 예시를 보자.

```
let animal = {
    eats: true,
    walk() {
        alert("동물이 걷는다.");
    }
};

let rabbit = {
    jumps: true,
    __proto__: animal
};

rabbit.walk(); // 동물이 걷는다.
```

Prototype인 animal에서 walk 를 자동으로 상속받았기 때문에 rabbit 에서도
walk 를 호출할 수 있다.

![image](https://user-images.githubusercontent.com/76507701/185426863-1d88880b-4b57-4d98-a967-e60e5e3e9ed9.png)

## 3. Prototype Chaining

위의 예시에서 longEar라는 객체를 추가하면, 다음과 같은 코드가 나온다.

```
let animal = {
    eats: true,
    walk() {
        alert("동물이 걷는다.");
    }
};

let rabbit = {
    jumps: true,
    __proto__: animal
};

let longEar = {
    earLength: 10,
    __proto__: rabbit
};

longEar.walk(); // 동물이 걷는다.
alert(longEar.jumps); // true
alert(longEar.nothing); //undefined
```

![image](https://user-images.githubusercontent.com/76507701/185449758-cc4ebffa-1996-49ad-8f88-8145ad33398e.png)

walk method는 rabbit 객체를 거쳐 animal 객체에서 찾을 수 있고,
jumps property는 rabbit 객체에서 찾을 수 있다.
하지만, 계속 상속하는 상위 객체를 찾다가 **null을 만나게 되면** 그 때 **undefined 값을 반환**하게 된다.

- 이렇게 모든 객체의 부모 객체로 접근하는 일련의 프로세스를 Prototype Chaining이라 부른다.

프로토타입 체이닝엔 두 가지 제약사항이 있다.

1. 순환 참조(circular reference)는 허용되지 않는다. `__proto__ `를 이용해 닫힌 형태로 다른 객체를 참조하면 에러가 발생한다.

2. `__proto__` 의 값은 객체나 null 만 가능하다. 여기에 더하여 객체엔 오직 하나의 [[Prototype]] 만 있을 수 있다. 객체는 두 개의 객체를 상속받지 못한다.

## 4. Prototype은 읽기전용

- **Prototype은 property를 읽을 때만 사용**한다. 프로퍼티를 추가, 수정하거나 지우는 연산은 객체에 직접 해야한다.

```
let animal = {
    eats: true,
    walk() {
        // rabbit객체는 animal의 walk method를 사용하지 않는다.
    }
};

let rabbit = {
    __proto__: animal
};

rabbit.walk = function() {
    alert("토끼가 깡충깡충 뛴다.");
};

rabbit.walk(); // 토끼가 깡충깡충 뛴다.
```

![image](https://user-images.githubusercontent.com/76507701/185452906-2c807faa-56ed-4d93-bb42-16e30fbf57a6.png)

rabbit.walk() 를 호출하면 Prototype인 animal 객체에 있는 method가 실행되지 않고, rabbit 객체에 직접 추가한 메서드가 실행된다.

하지만, 접근자 property는 setter 함수를 사용해 프로퍼티에 값을 할당하므로 접근
자 프로퍼티에 값을 할당하면 객체에 property가 추가되는게 아니라 **setter 함수가 호출**되면서 위 예시와는 조금 다르게 동작한다.

```
let user = {
    name: "John",
    surname: "Smith",
    set fullName(value) {
        [this.name, this.surname] = value.split(" ");
    },
    get fullName() {
        return `${this.name} ${this.surname}`;
    }
};

let admin = {
    __proto__: user,
    isAdmin: true
};

alert(admin.fullName); // John Smith
admin.fullName = "Alice Cooper"; // setter 함수가 실행된다.
alert(admin.fullName); // Alice Cooper
alert(user.fullName); // John Smith
```

`alert(admin.fullName);`는 setter에 의해 추가된 admin의 property에서 값을 가져오기 때문에 Alice Cooper가 출력된다.
하지만, `alert(user.fullName);`는 user에 있는 property의 값인 John Smith가 출력된다.

## 5. Prototype에서 this가 가리키는 것

위의 예시에서 this는 Prototype의 영향을 받지 않는다.
`admin.fullName=` 으로 setter 함수를 호출할 때, this는 user가 아닌 admin을 가리킨다.

- 즉, this 는 Prototype의 영향을 받지 않는다. method를 객체에서 호출했든 Prototype에서 호출했든 상관없이 this는 언제나 . 앞에 있는 객체를 가리킨다.

다음 예시를 보자.

```
let animal = {
    walk() {
        if (!this.isSleeping) {
            alert(`동물이 걸어간다.`);
        }
    },
    sleep() {
        this.isSleeping = true;
    }
};

let rabbit = {
    name: "토끼",
    __proto__: animal
};

rabbit.sleep();
alert(rabbit.isSleeping); // true
alert(animal.isSleeping); // undefined
```

// rabbit에 새로운 isSleeping property를 추가하고 그 값을 true로 변경한다.
rabbit.sleep() 을 호출하면 객체 rabbit 에 isSleeping 프로퍼티가 추가된다.
`alert(rabbit.isSleeping);`는 정상적으로 true가 출력되지만, animal객체에는 isSleeping라는 property가 없기 때문에 `alert(animal.isSleeping);`는 undefined가 출력된다.
즉, **this를 사용해 데이터를 바꾸면 animal 이 아닌 해당 객체의 상태가 변한다.**

![image](https://user-images.githubusercontent.com/76507701/185455313-ab5ab6c1-1c30-4d9e-9285-651077f0adf6.png)

## 참고 자료

Ilya Kantor.『코어 자바스크립트』
