## 다양한 함수의 표현식

#### 1. 즉시 실행 함수 (IIFE)

그게 뭐야?

```javascript
    (function() {
        console.log('hello');
    })();
````
함수 정의와 동시에 즉시 호출되는 함수를 즉시 실행 함수(IIFE, Immediately Invoked Function Expression)라고 한다.
즉시 실행 함수는 단 한 번만 호출되며 다시 호출할 수 없다.
이런식으로 사용한다.


왜 사용할까?

주로 한번만 실행되면 되는 함수에 사용한다.

주로 초기화 코드 부분에 많이 사용하게 되는데,

변수를 전역으로 선언하는 것을 피하기 위해 사용됩니다.

```javascript
    (function() {
        var foo = 10; // 지역 변수
        var bar = 2; // 지역 변수
        console.log(foo * bar); // 20
    })();
    console.log(foo * bar); // ReferenceError: foo is not defined
````

#### 2. 재귀 함수

    말그대로 자기 자신을 호출하는 함수를 재귀 함수라고 한다.
    재귀 함수는 반복문을 사용하지 않고도 반복적인 처리를 할 수 있다는 장점이 있다.
    하지만 재귀 함수는 호출 스택(Call Stack)을 사용하기 때문에 반복문에 비해 상대적으로 많은 메모리를 차지한다는 단점이 있다.
    따라서 재귀 함수는 호출 횟수가 적을 때나 스택의 메모리 한계를 넘지 않을 때 사용하는 것이 좋다.


#### 3. 중첩 함수

함수안에 함수가 있는 것.

```javascript
        function outer() {
            var x = 1;
    
            // 중첩 함수
            function inner() {
                var y = 2;
                // 외부 함수의 변수를 참조할 수 있다.
                console.log(x + y); // 3
            }
    
            inner();
        }
    
        outer();
````


#### 4. 콜백 함수

>    함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 한다.
>
>    콜백 함수를 전달받은 함수는 이 콜백 함수를 필요에 따라 적절한 시점에 호출한다.
>
>    콜백 함수를 전달받은 함수는 호출 시점을 결정하지 않고 단지 전달받은 콜백 함수를 호출할 뿐이다.
>
>    콜백 함수를 전달받은 함수는 호출 시점을 결정할 권한을 가지고 있다.
>
>    콜백 함수는 비동기 처리(Asynchronous processing)에 유용하게 사용된다.

```javascript
    function repeat(n, f) {
        for (var i = 0; i < n; i++) {
            f(i); // i를 전달하면서 f를 호출
        }
    }
    
    var logAll = function (i) {
        console.log(i);
    };
    
    // 반복할 때마다 서로 다른 동작을 하도록 콜백 함수를 전달한다.
    repeat(5, logAll); // 0 1 2 3 4
    
    var logOdds = function (i) {
        if (i % 2) console.log(i);
    };
    
    // 반복할 때마다 서로 다른 동작을 하도록 콜백 함수를 전달한다.
    repeat(5, logOdds); // 1 3
````



## 스코프

### 스코프란?

스코프는 식별자가 유효한 범위를 말한다.
스코프는 코드의 가장 바깥 영역부터 시작해서 코드가 중첩되어 있는 구문 블록 내부로 점차 좁혀지는데, 이렇게 식별자가 유효한 범위를 **스코프**라고 한다.


### 스코프의 종류

#### 전역스코프와 지역스코프

전역스코프는 코드의 가장 바깥 영역을 말하고, 지역스코프는 함수 몸체 내부를 말한다.


### 스코프 체인

스코프는 함수의 중첩 관계로 인해 스코프가 계층적으로 연결된 것을 말한다. 이러한 스코프의 계층적 구조를 스코프 체인이라 한다.

```javascript
    var x = 'global';

    function foo() {
        var x = 'local';
        console.log(x); // local
    }

    foo();

    console.log(x); // global
````

### 렉시컬 스코프

렉시컬 스코프는 함수를 어디서 호출했는지가 아니라 어디에 선언했는지에 따라 결정된다.
즉 함수가 호출된 위치는 상관하지 않고 함수가 선언된 위치에 따라 상위 스코프를 결정한다.

```javascript
    var x = 1;

    function foo() {
        var x = 10;
        bar();
    }

    function bar() {
        console.log(x);
    }

    foo(); // 1
    bar(); // 1
````


## 전역변수의 문제점

#### 변수의 생명주기

변수는 선언에 의해 생성되고, 할당을 통해 값을 갖는다.
일반적으로 함수 안에 선언된 변수는 함수가 종료되면 소멸된다.
하지만 클로저의 경우 어디선가 스코프를 참조하고 있다면 소멸되지 않는다.

```javascript
    function foo() {
        var x = 1;
        var y = 2;

        // 함수 bar는 클로저다.
        function bar() {
            console.log(x); // 1
            console.log(y); // 2
        }

        return bar;
    }

    var bar = foo();
    bar();
```



#### 전역변수의 문제점

*암묵적 결합*

전역변수는 모든 코드가 참조할 수 있기 때문에 어디서든지 읽고 쓸 수 있다.
이는 코드의 복잡성을 증가시키고 의도치 않은 상태 변경을 유발할 수 있다.

*긴 생명주기*
전역변수는 애플리케이션의 생명주기 동안 유지된다.
이는 메모리 리소스를 오랫동안 소비하게 되어 성능에 악영향을 미칠 수 있다.

*스코프 체인 상에서 종점에 존재*
전역변수는 전역 객체의 프로퍼티이다.
전역 객체는 모든 객체의 최상위에 존재하는 객체로서 모든 객체는 전역 객체의 프로퍼티를 참조할 수 있다.
이는 변수의 검색 속도를 느리게 하며 전역 변수의 사용은 변수의 검색 시간을 증가시킨다.

*네임스페이스 오염*
전역변수는 어디서든지 참조할 수 있기 때문에 다른 스크립트 파일, 라이브러리와 충돌할 수 있다.
이는 의도치 않은 결과를 유발할 수 있다.


전역변수는 위와 같은 문제점을 가지고 있기 때문에 사용을 억제해야 한다.

그래서 즉시실행 함수를 사용하거나, 네임스페이스 객체를 사용하여 전역변수를 최소화하는 방법을 사용한다.

```javascript
    var MYAPP = {};

    MYAPP.name = 'Lee';
    console.log(MYAPP.name); // Lee
```

이렇게 하면 전역변수를 최소화할 수 있다.


## let , Const 키워드와 블록 레벨 스코프

#### let 과 const 키워드

var 키워드로 선언한 변수는 함수 스코프를 갖는다. 
즉, 함수 몸체 내부에서 선언한 변수는 함수 코드 전체에서 유효하다.

하지만 let 키워드로 선언한 변수는 블록 레벨 스코프를 갖는다.
블록 레벨 스코프란 코드 블록이 만든 스코프를 말한다.
블록 레벨 스코프를 갖는 변수는 변수의 유효 범위가 블록의 중괄호{}로 둘러싸인 코드 블록 내부로 제한된다.

```javascript
    if (true) {
        let foo = 10;
        console.log(foo); // 10
    }
    console.log(foo); // ReferenceError: foo is not defined
```

let 키워드로 선언한 변수는 중복 선언이 금지된다.

```javascript
    let foo = 123;
    let foo = 456; // SyntaxError: Identifier 'foo' has already been declared
```

let 키워드로 선언한 변수는 “선언 단계”와 “초기화 단계”가 분리되어 진행된다.

```javascript
    console.log(foo); // ReferenceError: foo is not defined
    let foo;
```

일시적 사각지대(스코프의 시작 지점부터 초기화하기 전까지는 변수를 참조할 수 없는 구간)때문에 호이스팅이 발생하지 않는 것처럼 보이지만 호이스팅이 발생한다.

const 키워드는 let 키워드와 마찬가지로 블록 레벨 스코프를 갖는다.
하지만 const 키워드로 선언한 변수는 재할당이 금지된다.

```javascript
    const foo = 1;
    foo = 2; // TypeError: Assignment to constant variable.
```

const 키워드로 선언한 변수는 선언과 동시에 초기화해야 한다.

```javascript
    const foo; // SyntaxError: Missing initializer in const declaration
```


