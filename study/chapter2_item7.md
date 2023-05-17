# 📚 Chapter 2 - 타입스크립트의 타입 시스템

## 🪑 item7. 타입이 값들의 집합이라고 생각하기

### 타입은 할당 가능한 값들의 집합 또는 범위
모든 변수는 __런타임__에 자바스크립트 세상의 값으로부터 정해지는 각자의 __고유한 값__을 가진다.

- 타입은 __할당 가능한 값들의 집합__이라고 생각해야한다. 또는 __범위__라고 이해할 수도 있다.

<br/>

### never, 유니온 타입, & 연산자
- __never__: 가장 작은 집합. 공집합
- __유니온 타입__: 값 집합들의 합집합
- __& 연산자__: 두 타입의 인터섹션. 교집합

<br/>

### 자주 헷갈리는 부분 - 1
- 구조적 타이핑 규칙들은 어떠한 값이 다른 속성도 가질 수 있음을 의미한다. 👉 선언된 타입을 가졌고, 그 이상의 속성을 가지면 해당 타입에 할당할 수 있다.
- 특정 상황에서만 추가 속성을 허용하지 않는 __잉여 속성 체크(excess property checking)__ 때문에 사람들이 종종 타입의 범위를 오해하곤 한다.

<br/>

### 자주 헷갈리는 부분 - 2
- & 연산자가 유니온 타입과는 다르게 교집합을 의미한다는 점. & 연산자로 연결하면 두 개의 타입 범위에 모두 속할 수 있는 타입이 된다.

```
interface Person {
  name: string;
}
interface Lifespan {
  birth: Date;
  death?: Date;
}

type PersonSpan = Person & Lifespan;

const ps: PersonSpan = {
  name: 'Alan Turing',
  birth: new Date('1912/06/23'),
  death: new Date('1954/06/07'),
};  // OK
```

<br/>

### 서브 타입
- 서브타입이란 어떤 집합이 다른 집합의 부분 집합이라는 의미이다.
- `extends` 키워드를 사용하여 유니온 타입이나 리터럴 타입보다 훨씬 직관적으로 부분 집합의 관계를 표시할 수 있다.
```
interface Vector1D { x: number; }
// Vector1D 는 { x: number; }

interface Vector2D extends Vector1D { y: number; }
// Vector2D 는 { x: number; y: number; }

interface Vector3D extends Vector2D { z: number; }
// Vector3D 는 { x: number; y: number; z: number; }
```

<br/>

### 제너릭 타입에서 extends 키워드가 사용되는 경우

```
function getKey<K extends string>(val: any, key: K) {
  // ...
}
```

- 위의 예시와 같은 경우 K는 string이라는 전체 큰 범위 안에 있는 작은 부분집합임을 의미한다. 