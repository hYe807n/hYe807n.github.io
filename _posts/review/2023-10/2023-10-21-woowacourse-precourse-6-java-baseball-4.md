---
title: "[우아한 테크코스-프리코스 1주차] 숫자야구게임 Error 해결 과정"
layout: post
subtitle: "woowacourse precourse 6 - 1st week mission"
category: review 
tags: java woowacourse-precourse-6
published: true
future: true
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

**[우아한 테크코스 1주차 미션 숫자 야구 게임 GitHub Link](https://github.com/woowacourse-precourse/java-baseball-6)**

## 1. StringIndexOutOfBoundsException
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/34e17a89-66bd-43ad-a9fa-92d2a639c831)
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/571d576a-2021-4a85-bdea-425607736c95)
 
 <br/>

* `String index out of range: -1`
현재의 `scoreCalculator()` method 와는 다르게 매 반복 회차마다 `i++` 를 해주었고, strike 요소는 3 개의 숫자 string 요소에서 제거해주었다. 그럼 i의 값은 그대로 있어야 다음 요소를 확인할 수 있으므로 <U>strike 일 때 로직에 마지막에 `i--`를 해주었는데 여기서 문제가 발생</U>한다.
   * 첫 번째 숫자가 strike일 경우, 
number 정보를 담은 string의 첫 번째 요소가 사라지고, index 0번째 요소는 두 번째 숫자가 되므로 문제가 없지만, **문제는 i 값이 1 감소한 상태로 28 행에서 index를 통해 string 요소에 접근**한다. 만약 첫 번째 숫자가 strike 일 경우에는, index가 -1이 되므로 28 행에서 오류가 발생할 것이다.

### 수정 후
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/8105aaf7-a924-4ccb-accc-486938203204)

<br/>

매 회차마다 `i++` 해주는 것이 아닌, strike 가 아닐 경우(볼/낫싱)에만 `i++` 할 수 있도록 `continue` 문을 통해 수정해주었다.

<br/>
<br/>

