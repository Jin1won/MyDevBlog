### HTTP Methods

## 1. REST

- REST API는 웹에서 데이터를 전송 및 처리하는 방법을 정의한 인터페이스를 말한다.
- 모든 데이터 구조와 처리방식은 REST에서 URL을 통해 정의되며, 그래서 매우 직관적으로 이해할 수 있다.

## 2. HTTP Method 와 CRUD

### REST 구성

- 자원(resource): URI
- 행위(verb): HTTP Method
- HTTP Method를 통해 해당 자원에 대한 CRUD 연산을 적용하여 아래와 같이 사용한다.
  - Create: 데이터 생성 (POST)
  - Read: 데이터 조회 (GET)
  - Update: 데이터 수정 (PUT)
    - PUT : 리소스를 대체, 해당 리소스가 없으면 생성(덮어쓰기와 비슷)
    - PATCH : 리소스 변경(부분 수정)
  - Delete: 데이터 삭제 (DELETE)

### GET

- GET은 보통 조회를 할 때 사용한다.
- DB로 생각했을때는 SELECT에 해당

```
GET http://localhost:8080/user/{user_id}
```

### POST

- POST는 보통 데이터를 추가할 때 사용한다.
- DB로 생각했을때는 INSERT에 해당
- 보통 생성 과정이 성공적으로 끝나면, 응답값으로 201 CREATED를 보낸다.

```
POST http://localhost:8080/user

{
    "name": "jinwon",
    "age": "26",
 }
```

### PUT

- PUT은 데이터를 수정 할 때 사용한다.(덮어쓰기와 비슷)
- DB로 생각했을때는 UPDATE에 해당
- POST와 동일한 URL로 요청하지만, HTTP 메소드가 다르기 때문에 다르게 동작한다.

```
PUT http://localhost:8080//user/{user_id}

{
    "name": "jinwon"
}
```

### PATCH

- PATCH는 데이터를 수정 할 때 사용한다.(부분 수정)
- POST와 PUT과 동일한 URL이지만 다르게 동작한다.

```
PATCH http://localhost:8080//user/{user_id}

{
    "name": "jinwon"
}
```

### DELETE

- DELETE는 데이터를 삭제할 때 사용한다.
- DB로 생각했을때는 DELETE에 해당

```
DELETE http://localhost:8080/user/{user_id}
```

## 3. PUT vs PATCH

- PATCH와 PUT은 둘 다 데이터의 수정을 위한 method이다. 그렇다면 두가지는 어떤 차이점이 있을까?
  - PUT 요청 시 요청을 일부분만 보낸 경우 나머지는 default 값으로 수정되므로, 바뀌지 않는 속성도 모두 보내야 한다.
  - PATCH를 이용하여 특정 부분 변경하는 요청을 보내면, 새롭게 바뀐 부분만 반영되며 나머지는 기존의 데이터가 유지된다.
  - 즉, 자원의 일부를 수정할 때는 PATCH를, 전체적인 수정이 필요할 때는 PUT을 이용하는 것이 적절하다.

```
PUT http://localhost:8080/users/{user_id}
{
    "age": 26
}

HTTP/1.1 200 OK
{
    "name": null,
    "age": 26
}
```

- 위 코드에서 put 사용하여 age 를 변경할 때 수정된 값만 보내면 보내지 않은 데이터는 null로 변경되어 버린다.
- 따라서 put 요청시에는 변경되지 않는 데이터도 모두 전달해야 한다.

```
PUT http://localhost:8080/user/{user_id}
{
    "name": "jinwon"
    "age": 26
}

HTTP/1.1 200 OK
{
    "name": "jinwon",
    "age": 26
}
```

- 그러나 PATCH를 이용하여 age만 변경하는 요청을 보내면, 새롭게 바뀐 부분만 반영되며 나머지는 기존의 데이터가 유지된다.

```
PATCH http://localhost:8080/user/{user_id}
{
	"age": 26
}

HTTP/1.1 200 OK
{
	"name": "jinwon",
	"age": 26
}
```

## 4. 멱등성

- 동일한 요청을 한 번 보내는 것과 여러 번 연속으로 보내는 것이 같은 효과를 지니고, 서버의 상태도 동일하게 남을 때 해당 HTTP 메서드가 멱등성을 가졌다고 말한다.

> 멱등성 O : GET, HEAD, PUT, DELETE

> 멱등성 X : POST, PATCH

### 왜 PATCH 는 PUT 과 다르게 멱등성이 없을까?

- **PUT 는 전체를 덮어쓰기** 때문에 **매 요청에도 같은 결과**를 얻을 수 있다. 하지만 **PATCH** 의 경우 멱등성이 있다고 생각할 수 있지만 설계에 따라 멱등성이 없을 수도 있다. 예를 들어 일정 부분 교체하는 로직에서 **특정 필드의 값을 계속 10 씩 증가하게 설계**한다면 **매 요청마다 다른 결과를 얻을 수 있을 것**이다. 즉 설계에 따라 **멱등성이 없을 수 있기 때문에** PATCH 는 멱등성이 없다고 말한다.

## 참고 사이트

https://memostack.tistory.com/180#2.%20HTTP%20Method%EC%99%80%20CRUD

https://devuna.tistory.com/77
