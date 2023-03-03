## 리액트 테스트 라이브러리

### 학습 키워드

- React Testing Library
- given - when - then 패턴
- Mocking
- Test fixture



## 좋은 테스트의 구조 Given-When-Then
- Given : 준비
- When : 실행
- Then : 결과
  테스트 코드 템플릿.
> 700W 전자렌지를 준비해서 3분만 돌리면 완성!

코드로 보기
```
# Given : 준비, ( ~ 하고, ~ 할때 )
stove = Stove.new(700.watts)

# When :  실행,  (~ 하면 )
food = stove.cook(3.minutes)

# Then ~ : 결과값 확인,  (가 된다, ~를 한다.)
assert food.complete?
```

리액트 컴포넌트를 사용자 입장에 가깝게 테스트할 수 있는 도구이다.
freEvent 이벤트 쓸 때
jest.fn: 가짜로 mocking
mockClear


##  React Testing Library

```javascript
// 이벤트 쓰는법
fireEvent.change(screen.getByLabelText(label), {
  target: { value: 'New name'}
})

// setText 여부가 call 되었는지 
expect(setText).toBeCalledWith('New Name');


// 한번에 하나의 함수만 테스트해야 하고, 
// 만약 중복으로 사용하는 코드는 
// mockCleaer()
beforeEach(() => {
  setText.mockClear();
  // 또는 jest.clearAllMocks();	
});

```

1. 반복 되는 코드는 Extract Function한다.
2. fireEvent 등을 통해 인터렉션만 검증할 것




## Mocking

## Test fixture
