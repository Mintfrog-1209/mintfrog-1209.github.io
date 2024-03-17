---
layout: post
title: "[react-query] react-query의 기본 개념"
subtitle: "Atom과 Selector"
date: 2024-03-11 16:54:10 +09:00
background: "/img/posts/07.png"
published: true
---

# 1. 기존 상태관리 도구를 활용한 API 통신 및 비동기 데이터 관리의 문제점

## 1. 많은 양의 보일러플레이트 코드

이전 글에서 Redux의 단점을 설명할 때에도 언급한 내용이지만, Redux를 사용하는 데에는 많은 양의 보일러플레이트 코드들이 필요하다. 이는 redux-saga, redux-thunk 등의 Redux에서 비동기 데이터 처리를 도와주는 미들웨어들을 활용할때에도 마찬가지로, 이러한 구조에서는 API의 개수가 늘어날 수록 코드의 양 및 복잡성도 기하급수적으로 늘어나게 된다.

## 2. 규격화된 방식 부재

redux-saga 등의 미들웨어들을 활용하여 비동기 상태들을 불러오고 그 값들을 보관할 수는 있지만, 이를 활용한 구현은 전부 개발자의 몫이므로 개발자에 따라 데이터를 관리하는 형식과 방법이 달라질 수 있다. 이러한 형식과 방법에 정답은 없지만, 협업하는 개발자가 늘어나고 팀의 구조가 커지면 이를 조율하고 통일하는 것에 드는 시간도 자연스럽게 증가하게 될 것이고 효율의 문제를 낳게 될 것이다.

## 3. 데이터 캐싱, 최적화 등의 로직의 복잡함

프론트엔드에서 성능 최적화 및 사용자 경험 향상을 위해서는 데이터 캐싱 등을 활용하여 불필요한 데이터 요청의 횟수를 줄이거나 빠르게 데이터를 가져오는 것이 필수적으로 요구된다. 하지만 상태관리 라이브러리에서는 이러한 특화된 기능을 제공하지 않기 때문에 개발자가 해당 내용들을 직접 구현하게 되고 이는 개발 리소스를 소모하는 작업이 될 것이다.

이러한 문제점들을 해결하기 위해 react-query가 등장하였다.

# 2. React Query의 장점

## 1. 개발자에게 익숙한 Hook과 유사한 형태의 간단한방식의 API 요청

## 2. API 상태와 관련된 다양한 데이터를 제공하여 개발자가 직접 복잡한 구현을 하지 않아도 해당 기능들을 활용 가능

## 3. 캐싱 기능을 제공하여 불필요한 데이터 갱신으로 인한 성능 저하 방지

## 4. 오래된 데이터 업데이트, 동일한 데이터에 대한 중복 요청 방지 등 성능 최적화 기능 제공

## 5. 비동기 과정의 선언적 관리 가능 (Suspense, Error Boundary)

> 참고한 사이트 : [프론트엔드 아키텍처의 가장 최근 트렌드는?](https://yozm.wishket.com/magazine/detail/1663/),[카카오페이 프론트엔드 개발자들이 React Query를 선택한 이유](https://tech.kakaopay.com/post/react-query-1/#-api-%EC%9A%94%EC%B2%AD-%EC%88%98%ED%96%89%EC%9D%84-%EC%9C%84%ED%95%9C-%EA%B7%9C%EA%B2%A9%ED%99%94%EB%90%9C-%EB%B0%A9%EC%8B%9D-%EB%B6%80%EC%9E%AC),[react-query 개념 및 정리](https://kyounghwan01.github.io/blog/React/react-query/basic/),[Tanstack Query 공식 문서](https://tanstack.com/query/latest/docs/framework/react/overview)
