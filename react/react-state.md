# React State

## 학습 키워드 ✏️
- React state란?
- DRY 원칙
- SSOT(Single Source of Truth)
- useState
- 1급 객체(first-class object)란?
- Lifting State Up

## Thinking in react
- 3단계 : 상태를 찾고
- 4단계 : 어디에 둘 것인지 정한다음
- 5단계 : 데이터 흐름에서 역방향으로

## 3단계: UI state의 작지만 완전한 표현을 찾아라.
UI의 상호 작용은, 사용자가 기본 데이터 모델을 변경한다는 의미다.
기본 데이터 모델을 변경하려면 상태를 사용하고, 그 상태는 최소한으로 한다.

### React의 State
- 리액트의 state는 변경을 다루기 위한 요소이다.
- 데이터가 있다고 해서 모두 state가 되는 것은 아니다.
- state가 바뀌면 해당 컴포넌트와 하위 컴포넌트가 리랜더링된다.


- 중복 배제를 위해 DRY 원칙을 따르기
> DRY: 모든 논리 또는 지식이 단 하나의 추상화로 표현되어야 한다는 프로그래밍 원칙으로, 확장성의 핵심 요소 중 하나이다.
- 일관성을 위해 SSOT 단일 진실 공급원을 생각하기.
> SSOT: 정보와 스키마를 오직 하나의 출처에서만 생성 또는 편집하도록 하는 방법론.. 이라는데, 
> 이론적으로는 대략 무슨말을 하는지는 알겠는데 와닿지가 않다고 해야하나. 그냥 감을 못잡고 있다.

ex) 만약 필터를 하고 나면, 필터 된 결과를 상태로 굳이 두지 않음, 그냥 필터 된 결과를 바로 화면에 그려주도록 한다.
불필요한 상태를 만들게 되면(데이터를 중복을 가지고 있게 되면), 데이터가 바뀌었을 때 같이 바꿔주어야한다.

### 무엇이 상태인가? 상태를 찾아라.

React State의 조건:
1. 변경돼야 함. 변경되지 않는 건 state로 다룰 가치가 없다.
2. 부모 컴포넌트가 props를 통해 전달한다면 state가 아님.
3. `다른 state나 props를 이용해 계산 가능하다면 state가 아님`.

### 계산이 가능하면 state가 아니다라는 예시 코드.
여기서 categories 상태가 아니다.
기존 코드
```typescript
const ProductTable = ({ product }: ProductTableProps) => {
  const categories = selectCategories(product); // 상태 아님
  
  return ( ..// dosomthing )
}

```
주의 해야할 코드
```typescript
const ProductTable = ({ product }: ProductTableProps) => {
  const [categories, setCategories] = useState<string[]>([]);
  
  useEffect(() => {
    setCategories(selectCategories(product))
  }, [])
  
  return ( ..// dosomthing )
}
```
- 이렇게 한다고 작동을 하지 않는 건 아니지만 공식문서에도 useEffect사용을 자제하라고도 한다. [공식 문서 참조](https://beta.reactjs.org/learn/you-might-not-need-an-effect)
> useEffect에 상태가 의존하게 되면, 나중에 디버깅이 지옥에 빠질 수 있다고 한다. 명시적인 함수 호출로 대체하자.
- 타입스크립트를 잘 쓰면 관리하는 상태의 수를 줄여줄 수 있다.. (상태가 많을 떄는, 값을 하나씩 다룰 수 있는데 그 애들을 잘 묶어줄 수 있다.
타입스크립트를 잘 쓰면 모아줬을 때 어색하지 않음. 자바스크립트를 쓸 때는 오브젝트 안에 뭐가 들어있을지 모르기 때문에 불안하므로 props로 하나씩 던져주는 게 나은데, 
타입스크립트를 쓸 떄는 잘 정의된 타입을 가진 props하나만 던져주면 되니 편하고, 재사용할때 부담감이 없어진다 )

###  상태의 관리 및 소유자
- state 끌어올리기 ... (Lifting state up) !
- 함수를 넘겨도되고 setState로 넘겨도된다고 하는데, 대부분 함수로 넘기는 것을 선호 하는 것 같다.
- 어디까지? 공통으로 되는 곳이 있을 때까지 (기본적으로 제일 최상위에 두는 것 같다)

타입스크립트에서는 함수도 일급객체이다.
타입스크립트에서는 props가 많아지면, 묶어서 하나로 만들어도 된다. store라는 개념으로 많이 쓴다.
> 즉, 조건이 여러개일 경우 파라미터를 {} 오브젝트로 묶어서 전달
- 예시)
```typescript
// 사용하는쪽
const filteredProducts = filterProduct(products, { filterText, inStockOnly }).filter((product) => product.stocked)

// 쓰이는곳
type FilterConditiuons = {
 filterText: string 
 inStockOnly: boolean
}

const filterProducts = (products: Product[], { filterText, inStockOnly }: FilterConditiuons ) => {
    const filteredProducts = products.filter((product) => !inStockOnly || product.stocked); // 
    
    if (!filterText) return filteredProducts
  
    return filteredProducts.filter((product) => prodict.name.includes(filterText))
}
```

## 생각해봐야할 것
- 입력할때부터 trim() 설정
- 상태를 컴포넌트 안에 종속시켜도 되는걸까, 다른 곳으로 옮겨도 되지 않는가.
- 처음부터 대문자를 쓸때도 소문자로 바뀌게 하게
- setState를 왜하는가?
- 리액트에서 타입스크립트를 잘 쓰는법

### 공부중 
- DRY 
    - [참고1](DRY(Don't Repeat Yourself)는 코드의 모든 논리 또는 지식이 단 하나의 추상화로 표현되어야 한다는 프로그래밍 원칙)
- SSOT
- 일급 객체

### external Store?
- 같은 것들을 확실하게 책임을 넘겨주는 무언가를 만들어서 쓸 수 있다.
- 그것을 위해서 External Store쓴다.
- 리액트가 아닌 곳에서 상태를 관리하는 것을 external Store라고 부른다


