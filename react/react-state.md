# React State

## 학습 키워드 ✏️
- React state란?
- DRY 원칙
- SSOT(Single Source of Truth)
- useState
- 1급 객체(first-class object)란?
- Lifting State Up

## thinking in react
step3 4 5
- 3 상태를 찾아라
- 4 어디에 둘 건지 정하기
- 5 데이터 플로우에서 인벌스 하는거

## 상태
state가 바뀌면 해당 컴포넌트와 하위 컴포넌트가 리랜더링된다.
가능하면 DRY 원칙을 따라라 : 반복하지 마셈 (중복배제)
일관성을 위해 단일 진실 원천 ?? SSOT 단일 진실 공급원 
ex) 만약 필터를 하고 나면, 필터 된 결과를 상태로 굳이 두지 않음, 그냥 필터 된 결과를 바로 화면에 그려주도록 한다.

불필요한 상태를 만들게 되면(데이터를 중복을 가지고 있게 되면), 데이터가 바뀌었을 때 같이 바꿔주어야한다.

## react state의 조건

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
- 공식문서에도 useEffect사용을 자제하라고도 함.. [공식 문서 참조](https://beta.reactjs.org/learn/you-might-not-need-an-effect)
- 작동을 하지 않는 건 아니지만
- 타입스크립트를 잘 쓰면 관리하는 상태의 수를 줄여줄 수 있다.. (상태가 많을 떄는, 값을 하나씩 다룰 수 있는데 그 애들을 잘 묶어줄 수 있다.
타입스크립트를 잘 쓰면 모아줬을 때 어색하지 않음. 자바스크립트를 쓸 때는 오브젝트 안에 뭐가 들어있을지 모르기 때문에 불안하므로 props로 하나씩 던져주는 게 나은데, 
타입스크립트를 쓸 떄는 잘 정의된 타입을 가진 props하나만 던져주면 되니 편하고, 재사용할때 부담감이 없어진다 )


## 리액트에서 타입스크립트를 잘 쓰는법

## 상태의 관리 및 소유자
- 맨 최상위
- state 끌어올리기 ... (Lifting state up) !
- 함수를 넘겨도되고, setState로 넘겨도됨
- 어디까지? 공통으로 되는 곳이 있을 때까지

타입스크립트에서는 함수도 일급객체이다.
타입스크립트에서는 props가 많아지면, 묶어서 하나로 만들어도 된다. store라는 개념으로 많이 쓴다.

어떤 조건일때만, 필터되게끔 하려면
```


```
조건이 여러개일 경우 파라미터를 {} 오브젝트로 묶어서 전달
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
- 대문자를 쓸때도 소문자로 바뀌게 하게
- set을 왜하는가?


