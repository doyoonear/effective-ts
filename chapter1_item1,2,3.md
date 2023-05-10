# 📚 Chapter 1 - 타입스크립트 알아보기

## 🪑 item1. 타입스크립트와 자바스크립트의 관계 이해하기

### 타입스크립트는 자바스크립트의 상위 집합이다.
타입스크립트는 자바스크립트의 상위 집합이기 때문에 .js 파일에 있는 코드는 이미 타입스크립트라고 할 수 있다. 그리고 이러한 특성은 기존에 존재하는 자바스크립트 코드를 타입스크립트로 마이그레이션하는데 이점이 된다. 기존 코드를 그대로 유지하며 일부분에만 타입스크립트 적용이 가능하기 때문이다.

타입스크립트는 타입을 명시하는 추가적인 문법을 가진다. 그렇기 때문에 모든 자바스크립트 프로그램이 타입스크립트라는 명제는 참이지만, 그 반대는 성립할 수 없다. 
예를들면 `: string` 과 같은 문법은 타입스크립트에서만 쓰이는 타입 구문이다. 

타입스크립트 컴파일러는 타입 구문이 없는 자바스크립트 구문에도 유용하다.
<br/>

```
let city = 'new york';
console.log(city.toUpperCase());  
```

### 타입 추론

위 예시에서 타입 체커는 toUpperCase 부분에서 문제점을 찾아낸다. 위 예시에서 `city`에 따로 타입 구문으로 타입을 명시하지 않아도 __타입 체커는 초깃값으로 타입을 추론__하여 string 형식에는 toUpperCase 속성이 없다는 에러를 보여준다.

### 타입 구문을 통해 코드에 의도를 표시한다

이처럼 타입 시스템의 목표 중 하나는 __런타임에 오류를 발생시킬 코드를 미리 찾아내는 것__이다. 타입스크립트가 '정적' 타입 시스템이라는것은 바로 이런 특성을 말하는 것이다. 
타입스크립트는 타입 구문 없이도 오류를 잡을 수 있지만, 타입 구문을 추가한다면 더 많은 오류를 찾아낼 수 있다.
타입 구문을 통해 코드의 '의도'가 무엇인지 알려주는 것이다. 

그렇게 의도를 주입하게되면, 타입 체커를 통과한 타입스크립트 프로그램은 __자바스크립트의 런타임 동작이 '모델링'__된 결과물이라 볼 수 있다.
타입스크립트의 도움을 받으면 자바스크립트 코드가 의도하지 않은 동작을 하는 것을 방지할 수 있기 때문에 오류가 적은 코드를 작성할 수 있다.

<br/>
<br/>

## 🪑 item2. 타입스크립트 설정 이해하기

### 타입스크립트 설정 방법
1. command line 사용 시
- `tsc --noImplicitAny program.ts`

2. tsconfig.json 설정 파일을 사용하는 방법

```
  {
    "compilerOptions": {
      "noImplicitAny": true
    }
  }
```
<br/>

### 설정에서 가장 중요한 두 가지 - `noImplicitAny`, `strictNullChecks`

**`noImplicitAny`** <br/>
```
function add(a, b) {
  return a + b;
}

```
- 위 예시처럼 매개변수에 타입을 지정하지 않았을 때, 매개변수 a, b는 암시적으로 'any' 형식이 포함된다.
- 이러한 경우를 암시적 any(implicit any) 라고 부른다.
- 타입스크립트가 명시적인 타입 정보를 가질 때, 문제를 발견하기 쉬워지기 때문에 되도록이면 noImplicitAny를 설정해야한다.
<br/>

**`strictNullChecks`** <br/>

null과 undefined가 모든 타입에서 허용되는지 확인하는 설정이다.

```
const x: number = null;
```

- 위의 코드는 strictNullChecks가 해제되어있을때는 유효한 코드이고, 설정되어있을때는 오류가 난다.
- 'null' 형식은 'number' 형식에 할당할 수 없다는 에러가 발생한다. 

<br/>
<br/>

## 🪑 item3. 코드 생성과 타입이 관계없음을 이해하기

### 타입스크립트 컴파일러의 두가지 역할
1. 최신 타입스크립트/자바스크립트를 브라우저에서 동작할 수 있도록 구버전의 자바스크립트로 트랜스파일한다.
2. 코드의 타입 오류를 체크한다.

- 이 두가지가 서로 완벽히 독립적이라는 것을 이해하는게 중요하다. 즉 타입스크립트가 자바스크립트로 변환될 때 코드 내의 타입에는 영향을 주지 않는다.
- 그렇기 때문에 타입 오류가 있는 코드도 컴파일이 가능하다. 이는 장점이 있는데, 문제가 된 오류를 수정하지 않더라도 어플리케이션의 다른 부분을 테스트 할 수 있다.
- 오류가 있을때 컴파일하지 않으려면 tsconfig.json에 noEmitOnError를 설정하거나 빌드 도구에 동일하게 적용하면 된다.
<br/>

### 런타임에는 타입 체크가 불가능하다

```
interface Square {
  width: number;
}
interface Rectangle extends Square {
  height: number;
}
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
                    // ~~~~~~~~~ 'Rectangle' only refers to a type,
                    //           but is being used as a value here
    return shape.width * shape.height;
                    //         ~~~~~~ Property 'height' does not exist
                    //                on type 'Shape'
  } else {
    return shape.width * shape.width;
  }
}
```

instanceof 체크는 런타임에 일어나지만, `Rectangle` 은 타입이기 때문에 런타임 시점에 아무런 역할을 할 수 없다. 실제로 자바스크립트로 컴파일되는 과정에서 모든 인터페이스, 타입, 타입구문은 제거되어 버린다.

의도했던대로 `shape`의 타입을 명확하게 하려면 런타임에 타입 정보를 유지하는 방법이 필요하다. 

### 런타임에 타입 정보를 유지하는 방법

__방법 1: 'height' 속성이 존재하는지 체크해보는 방법__

```
function calculateArea(shape: Shape) {
  if ('height' in shape) {
    shape;  // Type is Rectangle
    return shape.width * shape.height;
  } else {
    shape;  // Type is Square
    return shape.width * shape.width;
  }
}
```
- 속성 체크는 런타임에 접근 가능한 값에만 관련되지만, 타입 체커 역시도 shape의 타입을 Rectangle로 보정해주기 때문에 오류가 사라진다.

<br/>

__방법 2: 런타임에 접근 가능한 타입 정보를 명시적으로 저장하는 '태그' 기법__

```
interface Square {
  kind: 'square';
  width: number;
}
interface Rectangle {
  kind: 'rectangle';
  height: number;
  width: number;
}
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape.kind === 'rectangle') {
    shape;  // Type is Rectangle
    return shape.width * shape.height;
  } else {
    shape;  // Type is Square
    return shape.width * shape.width;
  }
}

```
- Shape 타입은 태그된 유니온(tagged union)의 예이다. 이 기법을 사용하면 런타임에 타입 정보를 손쉽게 유지할 수 있다.
<br/>

__방법 3: 타입을 클래스로 만드는 방법__
```
class Square {
  constructor(public width: number) {}
}
class Rectangle extends Square {
  constructor(public width: number, public height: number) {
    super(width);
  }
}
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    shape;  // Type is Rectangle
    return shape.width * shape.height;
  } else {
    shape;  // Type is Square
    return shape.width * shape.width;  // OK
  }
}
```

인터페이스는 타입으로만 사용 가능하지만, Rectangle을 클래스로 선언하면 타입과 값으로 모두 사용할 수 있으므로 오류가 없다.