---
title: "[우아한 테크코스-프리코스 4주차] ERROR 해결 @ParameterizedTest ParameterResolutionException"
layout: post
subtitle: "woowacourse precourse 6 - 4th week mission"
category: review
tags: java woowacourse-precourse-6
published: true
future: true
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}



**[우아한 테크코스 4주차 미션 크리스마스 GitHub Link](https://github.com/woowacourse-precourse/java-christmas-6)**

# ParameterResolutionException
***
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/e80544e1-6332-4114-ba22-bddd700af8af)

[앞선 포스트에서 함수의 중복을 줄이기 위해서 단위테스트에서 @ParameterizedTest](https://hye807n.github.io/review/woowacourse-precourse-6-java-christmas(3).html)를 사용했다.
하지만 단위테스트 과정에서 `ParameterResolutionException`가 발생했다.
`@ParameterizedTest`는 Test 객체로서 동작하므로  `@Test` 와 `@ParameterizedTest` 가 동시에 사용되어서 발생하는 것이다. `@Test` 를 제거해주면 오류가 사라진다.
<br/>

![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/66c2e21b-e192-458b-9ec3-f8cfda0e5b12)
<br/>
<br/>

[참고자료]<br/>
* [문제해결: No ParameterResolver registered for parameter](https://youngwonhan-family.tistory.com/entry/%EB%AC%B8%EC%A0%9C%ED%95%B4%EA%B2%B0-No-ParameterResolver-registered-for-parameter)
* [@ParameterizedTest에도 각각의 이름을 부여할 수 있다??](https://programmer-ririhan.tistory.com/391)
* [[우아한 테크코스-프리코스 3주차] 단위테스트 코드 리팩토링](https://hye807n.github.io/review/woowacourse-precourse-6-java-christmas(3).html)