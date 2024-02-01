# week1(2024-02-01)
### 이종욱
```
// Q1 : 콘솔에 찍힐 값과 왜 그렇게 생각했는지 설명하시오.
function changeProperty(obj) {
obj.property = 'changed';
}
const myObj1 = { property: 'original' };
changeProperty(myObj1);
console.log(myObj1.property);// ?



// Q2 : 콘솔에 찍힐 값과 왜 그런지 설명하시오!
function changeReference(obj) {
obj.property = 'changed';
obj = { property: 'new' };
console.log(obj) //1번
}

let myObj2 = { property: 'original' };
changeReference(myObj2);
console.log(myObj2.property); // 2번
```
>  A1. 미리 정의된 함수에서 값을 변경하지만 참조에 의한 호출에 의해 myObj1의 원본 객체의 value가 변경된다.(메모리 주소를 인자로 받음)
>

>
> A2.1: { property: 'new' };
> `obj.property = 'changed'`로 인해 원본인 myObj2의 property는 changed로 변경 되었지만, `obj = {property : 'new'}`로 인해 obj에 새로운 메모리 주소를 가진 객체가 할당이 되어 원본을 가르키지 않기 때문에 원본 객체는 고쳐지지 않는다. 그렇기 때문에 1번 콘솔은 새롭게 할당된 객체가 출력된다.
>
> A2.2 : changed
>함수 안에서 `obj={property : 'new'}`가 선언되기 전에 원본 객체의 메모리 주소를 가르키고 있는 상태에서 값을 변경했기 때문에 changed로 property가 변경되었다. 

## 최재선
> Q. JS에서 함수는 표현식이 아닌 문임에도 불구하고, 특정 변수에 할당될 수 있는 것 처럼 작동하는 이유는?


> A. 자바스크립트 엔진이 문맥에 따라 동일한 함수 리터럴을 표현식이 아닌 문인 함수 선언문으로 해석하는 경우와, 표현식인 문인 함수 리터럴 표현식으로 해석하는 경우가 있기 때문이다.
>
> 따라서 위와 같은 경우는 function이 할당 연산자의 우변에서 평가되어야 할 문맥으로 판단되어, 객체 리터럴로 해석되는 것이다.

## 정미수
```
var x = 1;

function foo() {
var x = 10;
bar();
}

function bar() {
console.log(x);
}

foo(); // ?
```
> A. foo() 을 선언하면 전역변수로 선언된 x의 값이 10 으로 변경되는것이 아니라 다시 bar() 함수가 선언된다.
>
>이때 bar 에서는 지역변수 x가 존재 하지 않기 때문에 ,
>전역변수 x 가 실행된다. 따라서 답이 1 이다.
## 이나령
>Q1)var 키워드로 선언한 변수의 문제점
>
>Q2)함수 선언문이 변수에 할당되는 것처럼 보이는 코드가 동작하는 이유

>A1. 
> 1. 중복 선언 가능
> 2. 오로지 함수의 코드 블록만을 지역 스코프로 인정하므로 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언하더라도 모두 전역 변수가 된다.
> 3. 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환한다.
>
>A2. 자바스크립트 엔진이 문맥에 따라 함수 선언문으로 해석하는 경우와 함수 리터럴 표현식으로 해석하는 경우가 있다.