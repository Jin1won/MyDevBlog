## This

2022/09/05

- 자바스크립트의 함수는 호출될 때, this를 암묵적으로 전달 받고, 대부분의 경우 **this의 값은 함수를 호출한 방법에 의해 결정**된다.

### 1. 호출된 여러 상황에서 this가 가리키는 것

- **전역** 문맥에서 호출한 경우

  - this는 window 객체를 가리킨다.

- 함수를 **단순 호출**한 경우

  - strict모드가 아닌 경우에는 window객체를, strict 모드인 경우에는 undefined를 가리킨다.

  - 여기서 this를 다른 문맥으로 넘기고, 강제하려면 **bind, call, apply**함수를 사용하여 이를 처리할 수 있다.

- **화살표 함수**를 호출한 경우

  - 화살표함수에서 this는 **자신을 감싼 정적 범위**다. 전역 코드에서는 전역 객체를 가리킨다.

  - 화살표 함수를 call, bind, apply를 사용해 호출할 때 this의 값을 정해주더라도 무시한다.

- **객체의 메서드**로 함수를 호출한 경우

  - this의 값은 그 **객체를 가리킵니다**.

- **생성자**로 함수를 호출한 경우

  - 함수를 new 키워드와 함께 생성자로 사용하면 this는 **새로 생긴 객체에 묶인다**.

- **Event Handlers**로 함수를 호출한 경우

  - 함수를 이벤트 처리기로 사용하면 this는 **이벤트를 발사한 요소**로 설정된다.

### 2. bind, call, apply

**bind** 함수를 사용할 경우 this는 원본 함수를 가진 새로운 함수를 생성하는데, 새 함수의 this는 호출 방식과 상관없이 영구적으로 bind함수의 첫 번째 매개변수로 고정된다.

**call** 함수는 이미 할당되어있는 다른 객체의 함수나 메소드를 호출하는 해당 객체에 재할당할때 사용된다다. this는 현재 객체(호출하는 객체)를 참조한다. 메소드를 한번 작성하면 새 객체를 위한 메소드를 재작성할 필요 없이 call을 이용해 다른 객체에 상속할 수 있다.

**apply** 함수는 이미 존재하는 함수를 호출할 때 다른 this객체를 할당할 수 있다. this는 현재 객체, 호출하는 객체를 참조한다. apply 를 사용해, 새로운 객체마다 메소드를 재작성할 필요없이 한 번만 작성해 다른 객체에 상속시킬 수 있다.

call과 apply의 차이점은 call **인수 리스트**를, 반면에 apply는 **인수 배열 하나**를 받는다는 점이 중요한 차이점이다.

정리하자면

**bind는 호출은 하지 않고, this를 고정하고 call & apply는 호출까지 하고, 호출시마다 this를 바꿀 수 있다.**
