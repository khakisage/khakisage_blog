---
toc: true
layout: post
description: 기술 면접 개념 정리
categories: [infinite scroll, 가시성 관찰, 개념 정리]
title: Intersection Observer API
---

# Intersection Observer API

### 개발 배경

기존 `scroll` 이벤트와 **가시성 관찰**에 이용되는 `getBoundingClientRect`의 문제를 해결하기 위함.

- `scroll` 이벤트의 문제점 <br>
  스크롤 시 짧은 시간에 수 백, 수 천의 이벤트가 **동기적** 으로 실행될 수 있음.
  따라서, 실행되는 이벤트에 대한 리스너로 인하여 상응하는 콜백들이 메인 스레드에 큰 부하를 줄 수 있음.

- getBoundingClientRect의 문제점 <br>
  일반적으론, 여러 작업이 `queue`에 쌓여서 한 번의 `reflow`로 처리하지만, `getBoundingClientRect` 호출 시, 정확한 위치 정보를 파악하기 위해 여러번의 `reflow`가 발생됨.

### Intersection Observer

- Intersection Observer는 모든 영역을 사각형으로 취급함.(요소가 사각형이 아니더라도, 요소의 모든 부분을 픽셀과 같이 작은 사각형으로 가정하여 가시성을 계산함.)
  루트 요소와 타겟 요소의 교차점을 관찰하여 타겟 요소가 루트 요소에 들어가거나 벗어날 때, 혹은 두 요소의 교차부분(intersectionRatio)이 변경될 때, 지정한 콜백이 호출됨.
  즉, 기존 scroll 이벤트와 다르게 교차점이 관찰될 시, **비동기적**으로 실행되며, 가시성이 관측될 때마다 reflow가 발생하지 않아 성능면에서 유리함.

### 사용법

```js
// callback이 호출되는 상황(조건)을 정의. (root요소를 정의하지 않으면, default로 viewport가 root 요소로 설정됨.)
let options = {
  root: document.querySelector('#scrollArea'),
  rootMargin: '0px',
  threshold: 1.0,
};
// options에 따라 observer 인스턴스 생성.
let observer = new IntersectionObserver(callback, options);

// 타겟 요소 관찰 시작.
let target = document.querySelector('#listItem');
observer.observe(target);
```
