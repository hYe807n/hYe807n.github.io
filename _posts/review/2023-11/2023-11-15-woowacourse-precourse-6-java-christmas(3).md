---
title: "[우아한 테크코스-프리코스 4주차] 단위 테스트 코드 리팩토링 @Parameterized @CsvSource @ValueSource"
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

# 과제 진행 요구 사항 - 단위테스트
***
![과제 진행 요구사항](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/5214c181-4c3b-46d1-90fb-71d3383764e9)

3 주차 미션에 이어서 4 주차 미션에서 역시도 단위테스트 구현이 포함되어있었다.
**Java 의 클린코드 컨벤션에 따라 중복 코드, 중복 함수는 지양해야하고 이는 단위 테스트 코드에도 적용**되어야한다.
핵심 로직 코드가 중복되지만 몇 가지 인자나, 테스트 제목이 다른 경우에 중복되지 않기 위해 리팩토링 한 과정에서 사용한 기능을 보자.
<br/>
<br/>

# @ParameterizedTest
***
[이전 포스팅에서](https://hye807n.github.io/review/woowacourse-precourse-6-java-lotto(2).html) `@ParameterizedTest`의 기본적인 소개와 사용법은 적었으니 중복되는 내용은 생략했다.
코드가 중복돼서 하나의 함수를 구현하여 인자값을 받는 형태로 수정한다고 했을 때, 문제점이 하나 더 생긴다. 바로 Test 함수의 이름이 같아진다는 것인데, 놀랍게도 **`@ParameterizedTest` 에서도 name 인자를 지정해서 넘겨줄 수 있다는 것**을 이번 미션을 진행하며 처음 알게되었다.
<br/>
<br/>

## @ValueSource
```java
    @DisplayName("날짜가 1 미만이거나 31을 초과하거나 숫자 외의 문자를 포함하면 IllegalArgumentException 발생")
    @ValueSource(strings = {"0", "32", "2a"})
    @ParameterizedTest
    void OverRangeDateNumber(String number) {
        Assertions.assertThatThrownBy(() -> new Date(number))
            .isInstanceOf(IllegalArgumentException.class);
    }
```
테스트 함수 내에서 인자로 사용할 값들을 어노테이션을 통해 배열로 선언한다. 테스트를 실행하게 되면 배열을 순회하면서, 메소드 인자에 지정된 값들을 주입받고 테스트한다. 이 때, **하나의 테스트에는 하나의 인수(argument)만 전달**받을 수 있다.

<br/>
<br/>

## @CsvSource
```java
    @ParameterizedTest(name = "총혜택 금액이 {0}라면, 배지 {1}를 반환")
    @CsvSource({"5000, 별", "10000, 트리", "20000, 산타"})
    void checkBadgeByTotalPrice(int totalDiscount, Badges badge) {
        Assertions.assertEquals(Event.checkBadge(totalDiscount), badge.name());
    }
```
`@ValueSource` 와 유사하게 하나의 테스트에 사용할 값들을 `쉼표로 구분`하여 배열로 선언한다. 이는 CSV(Comma Sperated Value)라는 이름을 보면 쉽게 알 수 있다. 컴마 뿐만 아니라 커스텀 구분자를 사용할 수도 있다. 이는 `@CsvSource({"5000, 별", "10000, 트리", "20000, 산타"}, delimiterString="||")` 와 같이 지정하면 된다.

# Parameterized 적용 - 리팩토링 전후 비교
***
```java
    @DisplayName("총혜택 금액이 5000 원이라면, 배지 별을 반환")
    @Test
    void checkBadgeStar(){
        Assertions.assertEquals(Event.checkBadge(5000), Badges.별);
    }
    
    @DisplayName("총혜택 금액이 10000 원이라면, 배지 트리를 반환")
    @Test
    void checkBadgeTree(){
        Assertions.assertEquals(Event.checkBadge(10000), Badges.트리);
    }
    
    @DisplayName("총혜택 금액이 20000 원이라면, 배지 산타를 반환")
    @Test
    void checkBadgeSanta(){
        Assertions.assertEquals(Event.checkBadge(20000), Badges.산타);
    }
```

```java
    @ParameterizedTest(name = "총혜택 금액이 {0}라면, 배지 {1}를 반환")
    @CsvSource({"5000, 별", "10000, 트리", "20000, 산타"})
    void checkBadgeByTotalPrice(int totalDiscount, Badges badge) {
        Assertions.assertEquals(Event.checkBadge(totalDiscount), badge.name());
    }
```
함수 3 개의 핵심 코드가 중복되었던 기존과 다르게 `@Parameterized`, `@CsvSource`를 적용함으로서 함수 하나로 줄였다.
상황에 따라 `@NullSource, @EmptySource, @NullAndEmptySource` 등도 참고하여 활용해보면 테스트 코드의 전반적인 퀄리티 역시 상승시킬 수 있을 것 같다.
<br/>
<br/>
<br/>

[참고자료]<br/>
* [[JUnit5] @ParameterizedTest로 한 번에 테스트하자](https://velog.io/@ohzzi/junit5-parameterizedtest)
* [@ParameterizedTest에도 각각의 이름을 부여할 수 있다??](https://programmer-ririhan.tistory.com/391)
* [[우아한 테크코스-프리코스 3주차] 단위테스트 코드 리팩토링](https://hye807n.github.io/review/woowacourse-precourse-6-java-christmas(3).html)