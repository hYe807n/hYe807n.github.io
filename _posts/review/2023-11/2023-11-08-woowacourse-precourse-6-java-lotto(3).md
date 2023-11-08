---
title: "[우아한 테크코스-프리코스 3주차] for문, streamAPI forEach() 언제 사용해야할까?"
layout: post
subtitle: "woowacourse precourse 6 - 3rd week mission"
category: review
tags: java woowacourse-precourse-6
published: true
future: true
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}



**[우아한 테크코스 3주차 미션 로또 GitHub Link](https://github.com/woowacourse-precourse/java-lotto-6)**

# StreamAPI 
***
list나 array 와 같은 Collection를 사용한 모든 객체들은 StreamAPI 를 사용하는 것이 여러 측면에서 이득이다. 그래서 [이전 포스팅](https://hye807n.github.io/review/woowacourse-precourse-6-java-racing-car(5).html)에서 다루었듯이 여러 API 들을 사용해보았는데, 이 중에서는 for문과 같은 반복처리를 하는 `streamAPI` 도 포함되었다.
이번 미션에서도 가능하면 되도록 java API를 활용하기 위해 for문을 `stream.forEach()`로 대체하려고 했는데, 둘의 차이점을 알고싶었다.
하지만 모든 반복문을 `stream.forEach()` 로 대체하는 것은 바람직하지 않다는 점을 알게되었다. 그 이유를 보자.
<br/>
<br/>

### 성능 차이
[Angelika Langer가 컨퍼런스에서 발표한 글](https://devm.io/java/java-performance-tutorial-how-fast-are-the-java-8-streams-118830)에 따르면, Stream의 성능에 대한 내용이 있다.
일반적으로 성능에 치명적인 영향을 미치는 코드는 많지 않으나 `Stream.forEach()`는 전통적인 `for`문을 사용할 때보다 오버헤드가 훨씬 심각하게 발생한다.
모든 `for`문을 `Stream.forEach()`로 대체한다면, 누적되는 CPU 낭비가 심각해질 수 있다. 실제로 작은 사이즈부터 큰 사이트까지 테스트한 결과, 데이터의 양에 상관없이 시간이 배로 차이난다.
<br/>

> **`for <-> stream` 속도 차이가 나는 이유**
> 1. for문 - 단순 인덱스 기반
> * for문은 단순 인덱스 기반으로 메모리에 접근하지만, stream의 경우 JVM이 처리해줘야하는 것이 많아서 실행 시 느릴 수 밖에 없다.
> 2. for문 - compiler의 최적화
> stream은 java 8부터 지원된 반면, for문은 훨씬 이전부터 사용되어온 만큼 for 문에 대한 처리가 되어있어 최적화를 시킬 수 있다.
<br/>
<br/>

# stream.forEach() 사용할 때
***
for 반복문 코드 내에서 작성해야하는 로직이 있을 경우, streamAPI를 통해 간단하게 해결할 수 있다. 특히 filter, reduce, map 등의 기능을 사용할 때와 같은 경우에는 streamAPI를 사용함으로서 간단히 구현할 수 있으므로 용이하다.
하지만 단순히 반복만 하는 `forEach` 인 경우에는 for 문을 사용하는 것이 좋다.
<br/>
<br/>
<br/>

[참고자료]<br/>
* [for-loop 를 Stream.forEach() 로 바꾸지 말아야 할 3가지 이유](https://homoefficio.github.io/2016/06/26/for-loop-%EB%A5%BC-Stream-forEach-%EB%A1%9C-%EB%B0%94%EA%BE%B8%EC%A7%80-%EB%A7%90%EC%95%84%EC%95%BC-%ED%95%A0-3%EA%B0%80%EC%A7%80-%EC%9D%B4%EC%9C%A0/)
* [for문 보다 Stream API가 느리지만 사용하는 이유](https://pamyferret.tistory.com/49)