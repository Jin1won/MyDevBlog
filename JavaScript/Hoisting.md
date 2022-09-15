# Hoisting

2022/09/15

## 1. Hoisting?

- 코드 블록에 선언된 **변수선언 및 함수선언을 코드 블록의 최상단으로 끌어 올리는 것**을 말하며 실제로 끌어올려지지 않지만 옮긴 것처럼 동작하는 특성이다.

- 호이스팅의 종류에는 **var,let,const 호이스팅, 함수 선언식, 함수 표현식의 호이스팅이 있다. 호이스팅이 된다 하더라도 각각 차이점이 존재한다.**

## 2. var Hoisting

- **var**는 오로지 **함수의 코드 블록만을 지역 스코프로 인정**합니다. 따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 됩니다.

- 또한 var는 **재할당, 재선언이 가능**하며, 변수를 **선언하기 전에 참조할 수 있고**, 그 값은 undefined입니다.

예시

```javascript
function introduce() {
  alert(phrase); // undefined
  var phrase = "I'm jinwon";
}

introduce();
```

- **변수 선언**은 함수 실행이 시작될 때 **호이스팅되지만** **할당**은 **호이스팅 되지 않기** 때문에 할당 관련 코드에서 처리된다. 따라서 위 예시는 아래 코드처럼 동작하게 된다.

```javascript
function introduce() {
  var phrase;
  alert(phrase); // undefined
  phrase = "I'm jinwon";
}

introduce();
```

## 3. let,const Hoisting

- **let**은 var 와는 달리, **모든 코드 블록을 지역 스코프로 인정**한다. 또한, **재선언은 불가능하지만, 재할당은 가능하며, 선언 전에 사용하면** 초기화 되기 전까지 TDZ에 빠져 참조가 불가능하기 때문에 **ReferenceError**가 출력된다.

- **const** 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다. 즉, **재선언,재할당이 불가능**하다. let과 마찬가지로 초기화 되기 전까지 TDZ에 빠져 참조가 불가능하기 때문에 **ReferenceError**가 출력된다.

예시

```javascript
function introduce() {
  alert(phrase); // Uncaught ReferenceError: Cannot access 'phrase' before initialization at introduce
  let phrase = "I'm jinwon";
}

introduce();
```

## 4. var vs let/const

- 자바스크립트에서 변수를 메모리에 할당하는 과정은 크게 3단계로 진행된다.

- 첫번째로, 실행 컨텍스트 변수 객체에 등록하는 **선언 단계,** 두번째로, 변수 객체에 대한 메모리 할당 및 변수를 undefined로 초기화하는 **초기화 단계**, 마지막으로 undefined로 초기화 된 변수에 값 할당하는 **할당 단계**다.

- 각 호이스팅마다 **단계를 진행하는 방식이 다르기 때문**에 각각 차이가 존재한다.

- 앞서 설명한 변수의 메모리 할당 과정을 바탕으로 설명하자면, var는 1,2단계가 동시에 진행되면서 호이스팅이 된다.
  ![image](https://user-images.githubusercontent.com/76507701/190407472-ebcda741-50b0-4512-acee-2071bb78833c.png)

- 하지만, const/let은 1,2단계가 분리되어 진행되므로 1단계만 진행되면서 호이스팅이 되기 때문에 이런 차이가 발생한다. **호이스팅이 된 시점**을 봐보면 **var**의 경우 **변수에 undefined가 할당**되어 있는 상태고, **let/const**은 undefined로 **초기화되지 않았으므로 ReferenceError가 발생**한다.
  ![image](https://user-images.githubusercontent.com/76507701/190407619-d22922c1-6fc3-4be3-a8d6-5882d55d68dc.png)

## 5. 함수 선언식과 표현식 Hoisting

- **함수 선언식**의 경우 **함수의 선언이 호이스팅되어 선언 전에 사용이 가능**하다.

- **함수 표현식**의 경우 **변수 호이스팅이 발생**한다. 따라서, **var로 선언**되었을 때는 undefined이기 때문에 함수가 아니라는 **TypeError**가 발생하며, **let 또는 const**로 선언되었을 때는 TDZ로 인한 **ReferenceError**가 발생한다.

## 참고 자료

https://velog.io/@kich555/Lexical-Environment-Scope-TDZ-wqlit1i9

Ilya Kantor.『코어 자바스크립트』
