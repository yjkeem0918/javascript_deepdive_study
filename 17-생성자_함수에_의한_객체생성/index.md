# 생성자 함수에 의한 객체 생성

- 이번장에서는 생성자 함수를 사용하여 객체를 생성하는 방식을 살펴본다.
- 객체리터럴을 사용하여 객체를 생성하는 방식과 생성자 함수를 사용하여 객체를 생성하는 방식과의 장단점을 살펴본다.

## 17.1 Object 생성자 함수

new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체 완성할수 있다.

[예제 17-1]

```JS
// 빈객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function(){
    console.log('Hi! My name is '+ this.name);
};

console.log(person); // {name: 'Lee', sayHello: f}
person.sayHello(); // Hi! My name is Lee
```

생성자 함수(constructor)란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 **인스턴스(instance)**라 한다.
자바스크립트는 Object생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다.

=> Object 생성자 함수를 사용해 객체를 생성하는 방식은 특별한 이유가 없다면 `객체 리터럴`사용하는 것보다 유용해 보이지 않는다.

## 17.2 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하다. 하지만 단 하나의 객체만 생성한다.
따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다.

### 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위함 템플릿(클래스)처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

[예제 17-4]

```JS
// 생성자 함수
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

```

일반 함수와 동일한 방법으로 생성자 함수를 정의하고 **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.** 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```JS
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 가르킨다.
console.log(radius); // 15
```

### 생성자 함수의 인스턴스 생성 과정

- 생성자 함수의 역할
  1. 프로퍼티 구조가 동일한 인스턴스를 생성하기 위해 템플릿(클래스)으로서 동작하여 **인스텀스를 생성**
  2. 생성된 인스턴스를 초기화(인스턴스 프로퍼티 추가 및 초기값 할당)

자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환, new 연산자와 함께 생성자 함수를 호출하면 자바스크랍트 엔진은 다음과 같은 과정을 거쳐 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화한 후 암묵적으로 인스턴스를 반환한다.

1. 인스턴스 생성과 this 바인딩

   > `바인딩`: 식별자와 값을 연결하는 과정을 의미, 예를들어, 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. this 바인딩은 this와 this가 가리킬 객체를 바인딩하는 것이다.

2. 인스턴스 초기화
   생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.

3. 인스턴스 반환
   생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```JS
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Circle {}

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1. getDiameter: f}
```

명시적으로 return을 작성하면 명시한 객체가 반환되고, 원시 값을 반환하면 원시 값은 무시되고 암묵적으로 this가 반환된다.

=> 생성자 함수의 기본 동작을 훼손하므로 return 문을 반드시 생략해야한다.

### 내부 메서드 [[Call]]과 [[Consturct]]

todo: 16장 정리후 다시 읽기!
