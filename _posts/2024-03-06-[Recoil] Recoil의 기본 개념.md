---
layout: post
title: "[Recoil] 상태관리의 필요성"
subtitle: "Props Drilling과 상태관리"
date: 2024-03-06 16:48:10 +09:00
background: "/img/posts/05.png"
published: true
---

지난 글에서 상태관리의 필요성에 대해 알아보았다. 이번에는 Recoil이라는 라이브러리가 등장한 이유와 Recoil의 기본 개념을 알아보도록 하겠다.

# 1. Redux의 단점과 Context와 hooks

Redux는 이후 주류가 되었지만 조그마한 기능을 추가하는데도 너무 많은 보일러 플레이트(reducer, action, store 등)가 필요했다. 이는 대규모 프로젝트 이외의 중소규모 프로젝트에서는 오버엔지니어링이 되는 결과를 낳았다. 이후 React는 **Context**와 **Hooks**를 통해 조금 더 간결한 문법으로 **Props Drilling** 문제를 해결하면서 Redux의 입지는 점점 줄어들기 시작했다.

# 2. Recoil의 등장

하지만 Context와 Hooks도 단점이 없지는 않았습니다. Context와 Hooks를 사용해 상태를 공유하려면 **Provider**라는 공통 조상로 감싸야하는데, 이 경우 state가 갱신되면 state를 사용하지 않는 자식 컴포넌트까지 리렌더링이 발생하는 문제가 있었다. 이는 **UseMemo**와 같은 Hook을 추가로 사용하거나, Provider를 분리하는 것으로 피할 수 있지만, 그렇게 되면 코드가 복잡해지므로 Redux 대신 사용하는 이유가 사실상 사라지는 셈이 되었던 것이다.

따라서 **Atom**이라는 전역 객체를 활용해 데이터를 기록하고, View에서 Atom을 구독하는 방식의 **Recoil**이 등장하였다.

# 3. Atom, Selector

Recoil의 두가지 핵심적인 개념인 Atom과 Selector에 대해 알아보도록 하자.

## 1. Atom

**Atom**은 상태(State)의 단위이며, 업데이트(Update)와 구독(Subscribe)이 가능하다. Atom이 업데이트 되면 해당 Atom을 구독하고 있는 컴포넌트들은 리렌더링 된다. 동일한 Atom을 여러 컴포넌트에서 구독할 경우, 모든 컴포넌트는 해당 상태를 공유한다. Atom은 다음과 같이 정의할 수 있다.

```jsx
const balanceState = atom({
  key: "balanceState",
  default: 0,
});
```

Atom은 고유한 key값을 가져야 하며, 이는 디버깅이나, 특정 API 등에 사용된다. 또한 default 값을 통해 UseState와 같이 default value를 정해줄 수 도 있다.

컴포넌트에서는 **useRecoilState** Hook을 통해 불러올 수 있다. useState와 달리 서로 다른 컴포넌트에서도 같은 상태를 공유할 수 있다는 차이가 있다.

## 2. Selector

Selector는 Atom이나 다른 Selector를 인자로 받아들이는 순수 함수이다. 따라서 인자로 받은 Atom이나 Selector가 업데이트 되면 해당 Selector도 업데이트 되며, 해당 Selector를 구독하는 컴포넌트도 함께 리렌더링된다. Selector는 다음과 같이 정의할 수 있다.

```jsx
const wonBalanceState = atom({
  key: "wonBalanceState",
  get: ({ get }) => {
    const balance = get(balanceState);
    const wonRate = 1300;

    return balance * wonRate;
  },
});
```

Selector도 Atom과 같이 고유한 key값을 가지며, get 속성(Property)는 계산될 함수를 의미한다. get 인자(속성에 사용된 get과는 다르다)를 통해 다른 atoms와 selectors에 접근 할 수 있다.

이렇게 Selector를 활용하는 이유는 최소한의 상태만 Atom에 저장하고, 해당 Atom에서 파생되는 상태는 Selector를 활용함으로서 쓸모없는 상태를 만들고 보존하는 것을 방지한다. 위의 코드를 예시로 들면 달러 단위의 잔고(BalanceState)와 원 단위 잔고(wonBalanceState)의 상태를 각각의 atom으로 만드는 것 보다는 원 단위의 잔고는 달러 단위의 잔고에서 파생된 Selector로 만드는 것이 조금 더 효율적이라는 의미이다.

그런의미에서 Selector를 **파생된 상태(dervied state)**라고도 한다.

> 참고한 사이트 : [프론트엔드 아키텍처의 가장 최근 트렌드는?](https://yozm.wishket.com/magazine/detail/1663/),[Recoil 공식 문서](https://recoiljs.org/ko/docs/introduction/core-concepts),[Atoms, Selectors, 비동기 데이터 쿼리](https://velog.io/@js43o/Recoil-Atoms-And-Selectors)
