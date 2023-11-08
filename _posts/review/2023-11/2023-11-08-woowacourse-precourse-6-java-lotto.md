---
title: "[우아한 테크코스-프리코스 3주차] Enum"
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

# 과제 진행 요구 사항 - Enum 사용
***
![과제 진행 요구 사항](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/73daa568-40a6-4e4c-b5f2-6393b83ec616)

우아한 테크코스의 프리코스 3 주차 미션의 요구사항에 `Enum 사용`하는 것이 추가되었다. 하지만 Enum을 어디에 어떻게 적용하라는 언급은 되어있지 않았고, 이는 Enum 의 사용 목적을 직접 이해하여 어디에 적용해야할 지 판단해보라는 뜻으로 이해했다. 
<U>그렇다면 Enum이란 무엇일까?</U>
<br/>
<br/>

# Enum
***
Enum은 Enumeration의 약자로 "열거, 목록" 이라는 뜻을 가지고 있다. 먼저 C 에서 Enum은 상수들의 집합으로 사용하기 위한 용도로 기본적으로 정수로 처리되었다. 하지만 **Java 에서는 완전한 class 형 데이터로 취급**한다.
프로그래밍을 하다보면 `Array`, `List`, `String` 을 통해 데이터 주제에 따라 몇 가지의 연관된 데이터를 다루는 경우가 존재한다. 예를 들어 '요일'을 다룰 때 각각의 '월','화','수','목'.. 등의 연관된 데이터를 원시데이터가 아닌 상수로 정의하여 묶어서 사용할 수 있는데, **원시 데이터에 이름표를 붙여서 관련 작업끼리 묶어서 사용하는 것으로 이해하면 쉽다.**
<br/>

```java
public static final String RANK_1ST = "1 등";
public static final String RANK_2ND = "2 등";
public static final String RANK_3RD = "3 등";
```
Java 1.5 이하 버전까지는 Enum 사용이 지원되지 않아서 기존의 상수를 정의하는 방법이였던 `<Access Modifier> static final <datatype>` 를 사용하였다. 하지만 이런 방법으로 프로그래밍할 경우에 다양한 문제가 발생할 수 있고, 가독성이 떨어진다. 위의 예시 코드에서도 순위를 나타내기 위해 `관련있는 상수들의 접두어에 'RANK'를 사용하여 변수명이 한 눈에 알아보기 쉽지 않다.`
이를 보완하기 위해 새롭게 추가된 <U>Enum 을 통해 서로 연관된 상수들의 집합을 Class 로 관리</U>할 수 있다.
<br/>
<br/>

# Enum 사용법
***
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/8829b47e-2721-49b4-8d9e-cc08fb69f71f)
다음과 같이 별도의 enum class 를 생성하여 `.java` 파일을 생성한다. 
<br/>

## Enum 정의
```java
public enum Price {
    FIRST, SECOND, THIRD, FORTH, FIFTH
}
```
이처럼 단순하게 로또 등수를 열거한 Enum class 를 생성할 수 있다. **첫 번째 열거 데이터인 `FIRST`의 상수값은 `0`부터 설정되어, 차례대로 1 씩 증가된 값**을 가지고 있다.
하지만 **Java Enum은 상수를 객체지향적으로 관리하기 위함**이다. 독립된 class 로 구분하여 생성자, field 값과 관련된 기능을 함수로 정의하여 사용할 수도 있다.

* Enum 식별자는 class 와 같이 카멜 케이스로 선언한다.
* 열거 상수는 모두 대문자로 작성한다.
* 열거 상수가 여러 단어로 구성될 경우, 단어 사이를 `"_"` 로 연결한다.
</br>

## Enum 참조 방식
```java
Price first = Price.FIRST;
Price second = Price.SECOND;
```
Enum 은 하나의 `class` 이므로 Enum type 변수를 선언하여 할당한다. **Enum은 특수한 class로, `primitive` 타입이 아닌, `referece` 타입으로 분류하여 enum의 상수값을 Heap 영역에 저장**된다.

```java
first == Price.FIRST;
```
따라서 주소를 비교하는 연산자(`==`) 를 사용하여 비교하게 되면, `true` 가 결과로 나오게 된다.
</br>

## Enum 생성자, final field 선언
```java
public enum Price {
    FIRST(6, "6개 일치 ", 2000000000),
    SECOND(5, "5개 일치, 보너스 볼 일치 ", 30000000),
    THIRD(5, "5개 일치 ", 1500000),
    FORTH(4, "4개 일치 ", 50000),
    FIFTH(3, "3개 일치 ", 5000);

    private final int score;
    private final String guideline;
    private final int reward;

    public int getScore() {
        return score;
    }

    public String getGuideline() {
        return this.guideline;
    }

    public int getReward() {
        return reward;
    }

    Price(int score, String guideline, int reward) {
        this.guideline = guideline;
        this.reward = reward;
        this.score = score;
    }
```
만약 기본적으로 `0`부터 설정되는 정수값만이 아닌, 임의로 다른 값으로 설정하고, 다른 데이터를 함께 연관시켜주고 싶다면 **생성자를 통해 값을 할당**시켜줄 수 있다.
예시 코드에서는 각 등수마다 당첨 개수, 출력해야할 String, 상금 데이터를 생성자를 통해 객체로 연관시켜서 선언했다. 그리고 각 객체의 field 값을 선언하고 `getter` 를 통해 값을 활용할 수 있다.
<br/>

> #### Enum 의 private 생성자
> ![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/be60ba8e-11f7-478d-a496-599ff2d1b40f)
>
> Java 에서의 enum은 class 라고 말했는데, 일반 class 와 같이 생성자가 있어야한다. 하지만 기본적으로 생성자의 접근제한자는 **`private`으로 선언되어 있는데, `public` 이나 `protected`로 설정하게 되면 오류가 발생**한다.
> 그 이유는, **<U>enum type은 고정된 상수 집합이므로 런타임이 아닌 컴파일 타임에 모든 값을 알고 있어야한다.</U>** 즉 타 package 나 class 에서 enum 에 접근하여 동적으로 값을 할당할 수 없고, `new` 키워드 역시 사용할 수 없다. 이 때문에 생성자를 `private` 으로 설정하여 외부에서 접근할 수 없도록 하여 `instance` 가 없이 선언된 enum 상수를 존재하게 한다. 이러한 이유로 **싱글톤을 구현하는 방법으로 enum 을 적용**하기도 한다.

<br/>
<br/>

# Enum 사용 이점
***
### 상수값 타입안정성 보장
**enum은 컴파일 타임에 값이 정해지며 타입 안정성을 보장**한다. static이 생략되었을 뿐 enum <U>class 에 정의된 상수들은 static 변수 효과</U>를 나타내고 있고, 특정 범위까지의 값만을 사용하므로 컴파일 오류나 런타임 예외를 방지할 수 있다.
<br/>

### 코드의 단순화, 가독성
값들이 명시적으로 정의되어 있고, 외부에서 활용할 때에도 원시데이터가 아닌 `Price.FIRST` 와 같이 명시적으로 사용하므로 가독성이 좋다.
<br/>

### 상태와 행위를 한 곳에서 관리
enum class 에서 관련된 값을 field 값으로 지정하고, 관련 메서드를 정의하므로 관리가 용이하다. 값을 추가하거나 변경하는 경우, 한 곳에서 관리하므로 유지 보수가 용이하다.
<br/>

### 싱글톤 보장
앞서 말했듯, Enum 은 제한된 값 목록을 외부에서 접근할 수 없으므로 instance가 JVM 내에 하나만 존재한다는 것을 보장하여 싱글톤을 보장한다.
<br/>
<br/>

# 3 주차 미션 - Enum 적용 후기
3 주차 미션 요구사항 `Enum 적용`을 각 로또 당첨 번호 개수에 따라 등수를 나누는 것에 적용했다. 무작정 `Enum`을 적용하려고 하니, 막막해서 `Enum` 에 대한 이해 선수되어서 얻을 수 있는 이점을 알아야 어떻게 적용할 지 감이 잡힐 것이라고 생각했다. 
기존에 C, C++ 을 사용하면서 알고있던 `Enum`과는 다르게 Java 에서는 별도의 class 를 생성하여 관리한다는 점에서 관리하는 방법과 원리가 달라서 이해하기 위해 다양한 자료들을 참고하여서 메소드를 생성해보았다.
상수들로만 등수를 처리하는 것보다 별도의 class 내에서 관련된 기능들을 정의하기 때문에 사용하는 것에 용이함을 느꼈다.

<br/>
<br/>
<br/>

[참고자료]<br/>
* [Java: enum의 뿌리를 찾아서...](https://www.nextree.co.kr/p11686/)
* [☕ 자바 Enum 열거형 타입 문법 & 응용 💯 정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%97%B4%EA%B1%B0%ED%98%95Enum-%ED%83%80%EC%9E%85-%EB%AC%B8%EB%B2%95-%ED%99%9C%EC%9A%A9-%EC%A0%95%EB%A6%AC)
* [Java Enum 활용기](https://techblog.woowahan.com/2527/)
* [Enum 클래스](https://www.tcpschool.com/java/java_api_enum)
* [Java Enum 1편 : Enum 기본적인 사용](https://bcp0109.tistory.com/334)
* [[Java] Enum을 사용하는 이유와 사용법](https://velog.io/@mooh2jj/Java-Enum%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
* [[Java] Enum의 사용법](https://limkydev.tistory.com/66)