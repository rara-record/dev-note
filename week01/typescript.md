# TypeScript

간단하게 REPL을 사용하려면 ts-node를 사용해야 한다.

```shell
npx ts-node
```

## 타입지정

### 기본적인 타입 지정

```ts
let name: string;
let age: number;

name = 'yodoree';
age = '100'; // type error

let human: {
  name: string;
  age: number;
}; // obj 안에 타입이 정해지지 않은 새로운 프로퍼티나 타입이 다른 값을 넣으면 에러가 발생한다.
```

복잡한 오브젝트의 타입을 재사용할 수 있게 만들 수 있다.

`interface`와 `type` 두가지 방법이 있는데 아샬은 `type`을 선호한다.
interface는 과거부터 있었던 방법이다.
무슨 차이가 있는지 살펴보고 정리하자

```ts
type Human = {
  name: string;
  age: number;
};

interface Person {
  name: string;
  age: number;
}

const yodol: Human;
const dol: Person;
```

### 함수

```ts
function add(x: number, y: number): number {
  return x + y;
}
```

정해진 값으로 타입을 지정할 수도 있다.
`Union Type`에서 유용하게 사용된다.

```ts
let category: 'food';
category = 'food';
```

### 배열

```ts
let numbers: number[];
// 보다 엄격한 타입관리
let pair: [string, number];
```

### anything

```ts
let any: any;
```

### 타입추론

타입을 적어주지 않으면 값을 보고 타입을 잡아준다. 엄격하게 적어주면 좋을때가 있고 러프하게 적을때 좋은게 있다.

### Union Type

여러타입중에 고르는 것
예를들면 `true | false`
매개변수를 제한할때 유요하게 사용할 수 있다.

```ts
let y: true | false;

type Category = 'food' | 'toy' | 'bag';
let c: Category;

function fetchProducts({ category }: { category: Category }) {
    console.log(`Fetch ${category})
}
```

타입을 잡아놓으면 `vscode`에서 자동완성으로 뜬다.
또 레거시 환경등 기존 코드들 때문에 적어줘야하는 경우가 많다.

### Optional Parameter

```ts
function greeting(name?: string): string {
  return `Hello, ${name || 'world'}`;
}
// 기본값을 적어주는게 더 좋다.
function greeting(name: string = 'world'): string {
  return `Hello, ${name || 'world'}`;
}
```

매개변수가 오브젝트일 때도 활용할 수 있다.

```ts
function greeting({ name, age }: { name: string; age?: number }): string {
  return age ? `${name} ${age}` : name;
}
// 이렇게 작성하면 ts-node에선 해석불가
// 한줄로 작성해줘야함
```

이렇게도 작성 가능하다.

```ts
type Human = {
  name: string;
  age?: number;
};

function greeting({ name, age }: Human): string {
  return age ? `${name} ${age}` : name;
}
```

### Intersection Type

여러조건을 다 만족해야 한다.

```ts
type Human = {
  name: string;
  age: number;
};

type Creature = {
  hp: number;
  mp: number;
};

type Person = Human & Creature;

let person: Person;

person = { name: '홍길동', age: 13, hp: 256, mp: 16 };
```

### Generics, Utility Types, and Tips

`Generics`은 아직 잘 이해가 가지 않는다.
먼저 공부하고 다른 것들을 살펴보자.

### vscode 자동완성 + 실시간 에러 검사

현실적으로 `typescript`를 쓰는 가장 큰 이유는 vscode에서 자동완성을 지원하고 실시간으로 에러를 체크해준다.
코드작성하는 것에 있어서는 생각보다 불편하나
익숙해지면 훨씬 편리하게 코드를 작성할 수 있다.

### 읽어볼 자료

- [참고자료](https://velog.io/@lky5697/11-tips-that-help-you-become-a-better-typescript-programmer)

### 정리

typescript를 클론코딩하면서 사용해봣는데 제네릭등 사용에 어려움을 겪었었다. 그래서 any를 많이 활용하면서.. 사용했는데 그러면 의미가 없다라고 느껴졌는데 이번기회에 좀 더 명확하게 사용해 보자!
