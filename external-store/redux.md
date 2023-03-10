# Redux 따라하기

## 학습 키워드

- Redux
- Reflect

상태 변경 알림
스토어는 액션을 처리하고, 상태가 바뀌면 연결된 컴포넌트를 forceUpdate한다.

baseStore로  베이스 스토어를 생성 후 확장해서 사용한다.

```typescript tsx
import { container } from 'tsyringe';

import { useEffect, useState } from 'react';

import Store, { State } from '../stores/Store';

import useForceUpdate from './useForceUpdate';

type Selector<T> = (state: State) => T;

export default function useSelector<T>(selector: Selector<T>): T {
  const store = container.resolve(Store);

  const [state, setState] = useRef<T>(selector(store.state));

  const forceUpdate = useForceUpdate();

  useEffect(() => {
    const update = () => {
      const newState = selector(store.state);
      // 특정 조건이 맞으면 forceUpdate
      // selector의 결과가 객체인 경우 처리가 필요하다.
      if (newState !== state.current) {
        forceUpdate();
        state.current = newState;
      }
    };

    store.addListener(update);
    return () => store.removeListener(update);
  }, [store, forceUpdate]);

  return state;
}
```
