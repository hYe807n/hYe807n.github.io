---
title: "[우아한 테크코스-프리코스 2주차] 자동차 경주 게임 수정 사항(리팩토링 후)"
layout: post
subtitle: "woowacourse precourse 6 - 2nd week mission"
category: review
tags: java woowacourse-precourse-6
published: true
future: true
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}



**[우아한 테크코스 2주차 미션 자동차 경주 게임 GitHub Link](https://github.com/woowacourse-precourse/java-racingcar-6)**

# 리팩토링?
***
*본 포스팅은 우아한 테크코스 2주차 미션 자동차 경주 게임 1차 구현 코드를 기반으로 리팩토링 과정을 적었으므로 1차 구현 코드 리뷰와 겹치는 내용이 많아서 별도로 코드를 리뷰하지 않았다. 코드 이해가 어렵다면 1차 코드를 먼저 보고 오자 !*
[[우아한 테크코스-프리코스 2주차] 자동차 경주 게임 구현(리팩토링 전) 포스트 보러가기](https://hye807n.github.io/review/woowacourse-precourse-6-java-racing-car(3).html)

[자동차 경주 게임 최종 구현 코드 GitHub Link](https://github.com/hYe807n/java-racingcar-6/tree/hYe807n)
<br/>

1차 구현 코드를 보고난 후에 우선 얼핏 보기에도 수정해야할 것 같은 부분들이 보였다. 또한, 바로 체크하지 못한 부분들도 `README.docs` 의 요구 사항, 컨벤션, 1 차 공통 피드백을 통해서 수정할 점을 찾아나갔다.
객체지향성을 위반하여 코드를 구현하지는 않았는지, 다른 분들의 코드와 비교하기도 하며 코드 퀄리티를 높이기 위해 노력했다. 
<br/>
<br/>

# 수정사항
***
다음은 1 차 구현 이후에 수정한 사항을 크게 정리해보았다. *(문론, 1 차 구현에서도 컨벤션과 요구 사항을 신중히 고려했다.)*
<br/>

### 1. 일급 컬렉션(First-class Collection) 적용
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/92d6b18b-efed-4014-a78c-682c9eb72c71)

> CarController를 통한 Car 객체 관리 🔄 Car 객체 일급 컬렉션

<U>TDD, 클린 코드를 작성하기 위해서는 일급 컬렉션을 사용하는 것이 유용</U>하다는 것을 알게되었다. 일급 컬렉션에 대해 공부한 내용은 [이전에 작성한 포스트](https://hye807n.github.io/review/woowacourse-precourse-6-java-racing-car(2).html)에서 다루었으니 확인해보면 된다.
<br/>

### 2. Java API, Collection 적극 활용
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/5e8d894e-8f50-411c-97ba-ab3ddf758b9a)
> 반복문을 활용한 구현 🔄 Collection, Stream() 활용

일반 반복문과 함수로 직접 구현했던 로직을 1 차 공통 피드백 사항이였던 `Java API/Collection` 을 최대한 활용하도록 수정했다. Collection에 대해 공부한 내용 역시 [포스팅으로 작성](https://hye807n.github.io/review%20devlog/woowacourse-precourse-6-java-racing-car(5).html)해놓았다.
<br/>

## 3. JAVA 컬렉션 - break 사용 지양
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/aa997d6b-4ce9-499b-9ba2-ecc9dba434f0)
> `break` 문 사용 🔄 `break` 문을 사용하지 않는 로직

1 차 구현에서 최대 위치값을 구하기 위한 로직을 구현하기 위해서 `break` 문을 사용하였는데, **JAVA 컨벤션에 따라 `break` 문을 사용하지 않는 로직**으로 수정하였다. 
<br/>

## 4. Model, View 분리
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/fa0bfb20-c4bf-4fc4-98df-51463998352a)

> Model 객체에서 View 정보 활용 🔄 Model <-> View 독립적으로 분리

1 차 구현에서 Car 모델은 `OutputView` 에서 사용하기 위한 `"-"` 출력 양식을 알고 로직을 구현하였다. 하지만 **Model과 View 는 서로의 정보를 모르는 독립적인 개체여야 하므로 로직을 수정**하였다.
<br/>

## 5. 메서드 기능 중복 수정
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/dbc99c39-6798-4f34-9417-ca8ed0e093af)
> 같은 기능을 하는 메서드 중복 🔄 하나의 메서드로 활용

`InputView` 에서 정의한 함수 두 개는 기능과 형식이 전부 중복되는데, 두 개로 정의하였다. 그래서 두 가지 상황에 전부 쓰일 수 있는 **하나의 메서드로 중복을 방지**했다.
<br/>

## 6. 역할에 맞는 책임 부여
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/0bd7545d-8f67-4278-96b9-ac97c1c97101)
> Exception Class를 통한 validation 🔄 validation을 검증할 Car 객체로 책임 부여

`Exception` class 를 통해서 검증했었는데 어차피 이는 CarModel을 생성할 떄 Car만 해주면 되는 행위이므로 **CarModel이 수행할 수 있도록 책임을 위임**하였다.
<br/>

## 7. 객체지향 은닉화
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/7d250d58-0efd-4902-a4af-1b0167bde7b2)
> * 외부에서 Car 객체 상태 변경 🔄 Car 객체 내에서 판별하여 상태 변경
> * 접근제한자 `private` 적극 활용

기존에는 Car 객체 외부에서 전진할 지를 판단하여 객체 상태를 바꿀 수 있도록 했는데, 이를 객체지향의 특성인 은닉화를 보장할 수 있도록 **Car 객체에게 값을 전달하여 전진할 지 판단**할 수 있도록 수정하였다.
같은 이유로 **접근제한자를 되도록 `private`** 으로 설정하였다.
<br/>

## 8. 가독성 증가
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/a24aac97-3b29-48a3-9495-378c8c57b9c1)
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/869a3fa8-e6c0-44f4-ac9f-bc043219b8f4)
> * 원시 데이터 🔄 상수
> * `Ctrl+Alt+L` : 코드 자동 정렬
> * 함수 및 변수의 식별자 수정 

타인이 보아도 **변수와 함수의 의도를 알 수 있도록** 하고 가독성을 높이기 위해 원시 데이터 사용을 지양하고 상수로 선언하였다. 
또한 1 차 공통 피드백에서 언급 되었던 코드 자동 정렬 기능을 적극 활용했다.
그리고 중간 중간 함수 및 변수의 식별자가 역할에 맞게 지어졌는지 확인하고, 가장 역할에 적합한 식별자를 붙이기 위히 고민했다.
<br/>

## 9. 기능 목록 수정
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/dbfccd8e-94af-46c6-8a9a-d5314f01e0ca)
> * View, Model, Controller 등 자세한 설계와 기능 목록 🔄 필수 기능, 로직 목록 위주

기존의 기능 목록은 **너무 자세하게 적은 경향이 있어서 구현하는 과정에서 항상 사전에 구상한 대로 돌아가지는 않기 때문에 수정할 일이 너무 잦았다.** 때문에 필수적인 <U>기능 목록과 로직 위주로 정리하여서 핵심만</U> 알차게 적으려고 노력했다.

## 10. 단위테스트 수정
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/308b1d00-8f1a-4a3b-ad10-1c8575839c43)
> * 객체의 상태를 외부에서 활용 🔄 객체에게 메세지를 전달하여 행위하도록 함
> * 하나의 경우만 고려하여 테스트 🔄 여러 상황을 고려하여 테스트

기존에는 `랜덤 값이 4 이상이면 전진 하는 기능`을 하는 메서드를 만들었으면, 전진을 제대로 하는지만 테스트 했다면 4 이상이 아닐 때 정지도 제대로 하는지 한 가지만이 아니라 여러 상황에 대하여 테스트를 하도록 수정했다.



<br/>
<br/>

# 리팩토링 필요성
***
수정 사항이 자잘자잘하게 더 있지만.. 너무 사소한 것들은 적지 않았다. 분명 1 차 구현 과정에서도 1차 피드백 사항, 자바 컨벤션을 모두 충분히 숙지하고 고려하면서 구현했다. 특히 **`Indent 2 이상 금지`, `하나의 메소드의 하나의 기능만(하나의 메서드 구현부 15 줄 이상 지양)`** 을 지키기 위해 기능을 생성할 때 최대한 작은 단위로 쪼개서 생각해보고, 책임이 누구에게 있는 지 고민하는 시간을 보냈다. 
처음 1 차 구현을 시작할 때만 해도 '많이 구상하고, 규칙을 고려해가며 코드를 작성했기 때문에 리팩토링을 꼭 하지 않아도 되지 않을까..?' 내심 생각했는데 **코드의 퀄리티를 높이기 위해서 리팩토링 과정은 필수적**이라는 것을 깨달았다.

<br/>
<br/>
<br/>

[참고자료]<br/>
* [Java 리팩토링(Refactoring)의 7가지 방법과 예제](https://woogong80.tistory.com/213)
* [0118 추상화.캡슐화.은닉화.상속성.다형성-객체지향 개발](https://beerntv.wordpress.com/2017/01/18/0118-%E1%84%8E%E1%85%AE%E1%84%89%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA-%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%89%E1%85%B2%E1%86%AF%E1%84%92%E1%85%AA-%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%82%E1%85%B5%E1%86%A8/)
* [[우아한 테크코스-프리코스 2주차] Collections, Stream 적용](https://hye807n.github.io/review%20devlog/woowacourse-precourse-6-java-racing-car(5).html)
* [[우아한 테크코스-프리코스 2주차] 일급 컬렉션 적용하기](https://hye807n.github.io/review/woowacourse-precourse-6-java-racing-car(2).html)
* [완벽정리! Junit5로 예외 테스트하는 방법](https://covenant.tistory.com/256)
* [[Java] JUnit 가지고 놀아보기](https://bepoz-study-diary.tistory.com/319/)
* [[Java] JUnit을 활용한 Java 단위 테스트 코드 작성법 (2/3)](https://mangkyu.tistory.com/144/)
* [AssertEquals(String, String) ComparisonFailure when contents are identical](https://stackoverflow.com/questions/36324452/assertequalsstring-string-comparisonfailure-when-contents-are-identical)