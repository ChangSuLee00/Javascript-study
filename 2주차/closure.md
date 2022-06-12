# 클로저(CLOSURE)

> 클로저는 함수와 그 함수가 선언왼 렉시컬 환경(Lexical envirnoment)과의 조합
--  함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 특성 

> 자유변수에 묶여있는 함수

## 클로저 예시1

```javascript
const x = 1;

function outerFunc() {
  const X = 10;

  function innerFunc() {
    console.log(x); //10
  }

  innerFunc();
}
outerFunc();
```

## 클로저 예시2
```javascript
const x = 1;

function outerFunc() {
  const X = 10;
  innerFunc();
}

function innerFunc() {
   console.log(x); //1 
}

outerFunc();
```

[예시1]Outer함수 내부에서 inner함수를 생성하면 이를 중첩함수라 한다. 중첩함수는 자신을 포함하고 있는 외부함수 Outer함수의 변수에 접근할 수 있다.
[예시2]이와는 반대로 각 함수가 별개로 선언되었을 경우 inner함수를 Outer 함수 내부에서 호출하였다 하더라도 Outer함수의 변수를 사용할 수 없다.

## 렉시컬환경(Lexical Environment)

> 함수를 어디서 정의했느지에 따라 함수의 상위 스코프를 결정한다.
렉시컬 스코프 또는 정적스코프라 한다. 

```javascript
bar x = 1;
function foo(){
  var x = 10;
  bar();
}

function bar(){
  console.log(x);
}

foo();//1
bar();//1

```

bar함수는 전역세어 생성된 함수이고, 호출된 위치와 상관없이 전역 코드가 실행되기 전에 먼저 평가되어 함수 객체를 생성한다.

> 모든 함수는 [[Environment]] 라는 내부 프로퍼티를 갖고 있다. 이 프로퍼티는 함수가 만들어질 때 그 함수를 둘러싼 외부 렉시컬 환경에 대한 참조를 저장한다.

## 중첩함수 클로저
```javascript
const x = 1;

function outer(){
  const x = 10;
  const inner = function(){
    consoel.log(x);
  }
  return inner;
}

const innerFunc = outer();
innerFunc();
```

외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩함수는 생명주기가 종료함 외부 함수의 변수를 참조할 수 있다.

## 클로저가 아닌경우 

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다. 하지만 일반적으로 모든 함수를 클로저라고 하지 않는다.

- 상위 스코프의 식별자를 참조하지 않는 경우
- 상위 스코프의 식별자를 참조하지만 외부함수보다 중첩함수의 생명주기가 더 짧은 경우

## 클로저의 사용 

### 정보은닉 

> 클로저를 사용해 상태가 의도치 않게 변경되지 않도록 은닉한다. 또 특정 함수에게만 상태 변경을 허용한다.

```javascript
function matkCounter(aux){
  //자유변수
  let counter = 0;
  //클로저반환
  return function (){
    //보조함수에 상태 변경동작을 위임한다.
    counter = aux(counter);
    return counter;
  };
}
//보조함수
function increase(){
  return ++n;
}

//보조함수
function decrease(){
  return --n;
}

const increaser = new makeCounter(increase);
console.log(increaser());
console.log(increaser());

const decreaser = new makeCounter(decrease);
console.log(decreaser());
console.log(decreaser());
```

함수 샐행 컨텍스트는 소멸되지만 컨텍스트의 렉시컬 환경은 반환한 함수의 내부슬롯에 의해서 참조되고 있기 때문에 소멸되지 않음

### 캡슐화 

> 클로저를 사용해서 접근제한자(public, private, preotected) 기능을 하는 방법을 통해 정보은닉을 구현할 수 있다.
```javascript
const Person = (function(){
  //자유변수
  let age = 0;
  //생성자함수
  fucntion Person(name, page){
    this.name = name;
    age = page;
  }

  Person.prototype.sayHi = function(){
    console.log('${this.name},${age}');
  }

  return Person;
}());

const me = new Person('Kim', 29);
me.sayHi();//Kim 29
console.log(me.name);//Kim
console.log(me.age);//undefined

const you = new Person('Lee', 30);
you.sayHi();//Lee 30
console.log(you.name);//Lee
console.log(you.age);//undefined
```
해당 소스에서 SayHi는 실행이후 소멸하고 자유변수에 묶여있는 age만 렉시컬 환경에 남게된다.



## 참고

- [모던 자바스크립트 Deep Dive]
- https://developer-alle.tistory.com/
