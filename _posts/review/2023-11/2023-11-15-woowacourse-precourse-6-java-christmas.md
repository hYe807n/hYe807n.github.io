---
title: "[우아한 테크코스-프리코스 4주차] String.matches() 사용해보기"
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

## String.matches() 
***
`boolean String.matches(string string)`
파라미터로 받은 **정규표현식을 통해 해당 문자열 내부의 패턴 일치 여부를 확인하여 `true` 혹은 `false`를 반환**한다.
정규표현식을 사용하지 않아도 가능하지만 **<U>정확히</U>** 일치해야 `true`를 반환한다.
<br/>
<br/>

### 정규표현식
***
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/efe8b6b0-30cb-4994-a139-1d2ed53ecbcb)

정규 표현식은 특정한 규칙을 가진 문자열의 집합을 표현하는데 쓰이는 형식이다. 정규표현식을 사용하여 `전화번호`*(숫자 3 개-숫자 4 개-숫자 4 개)*, `주민등록번호`*숫자 6 개-숫자 7 개*,  등의 특정 패턴을 표현하고 해당 패턴과 일치하는 지 확인할 수 있다.
<br/>

### 예제 코드
```java
String bigIntro = "ABC";
System.out.println("ABC matches \"^[A-Z]*$\" : " + intro.matches("^[A-Z]*$"));

String smallIntro = "Aabc";
System.out.println("Aabc matches \"^[A-Z]*$\" : " + intro.matches("^[A-Z]*$"));
```

![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/3b96af47-63a6-472e-86d3-a50d9b6a123d)
<br/>

다음 코드는 `"^[A-Z]*$"`은 정규표현식을 통해 처음(^)부터 끝($)까지 0 개 이상(*)의 문자가 대문자 알파멧([A-Z])으로 구성되어있는지를 표현한 것이다.
따라서, `bigIntro`는 대문자로만 구성되어 있어서 `matches()` 함수를 적용했을 때 `true`를 반환하고 `smallIntro` 는 대문자가 포함되었지만, 소문자도 포함되어 있으므로 대문자로만 표현된 정규표현식으로 검사했을 때 `false` 를 반환하는 결과를 보여준다.

<br/>
<br/>

## `contains()`, `matches()` 차이점
***
```java
String intro = "Hello, world!";
String checkContains = "world";
String checkMatches1 = "[a-zA-Z]+, [a-z]+!";
String checkMatches2 = "[a-zA-Z]+[a-z]+";

System.out.println(intro.contains(check));  // contains() 사용 -> "true"를 출력
System.out.println(intro.matches(checkMatches1));    // matches() 사용 -> "true"를 출력
System.out.println(intro.matches(checkMatches2));    // matches() 사용 -> "false"를 출력
```
`contains()` 함수도 문자열을 다룰 때 사용되는 함수이다.
하지만 예시와 같이 패턴이 아닌, 단순히 주어진 문자열(`checkContains`)이 해당 문자열(`intro`) 안에 포함되어 있는지 여부를 확인한다.
<br/>
<br/>

## 코드에 `matches()` 적용 
***
```java
    private static final String FORM_ORDER = "^([가-힣]{3,8}-[0-9]+)(,[가-힣]{3,8}-[0-9]+)*$";
    private final List<Order> orders;

    public Orders(String orders) {
        validateForm(orders);   // 입력받은 주문 `orders`의 형식 validation 
        List<String> orderName = parseName(orders);
        List<Integer> orderCount = parseCount(orders);

        validateArgument(orderName, orderCount);
        this.orders = IntStream.range(0, orderName.size()).mapToObj(
            index -> new Order(orderName.get(index), orderCount.get(index))
        ).collect(Collectors.toList());
    }

    private void validateForm(String orders) {
        if (!orders.matches(FORM_ORDER)) {  // 형식이 일치하지 않는다면 오류 발생
            throw new IllegalArgumentException();
        }
    }
```
미션 과정 중, 입력받은 주문 내역의 형식은 `<메뉴이름>-<개수>,<메뉴이름>-<개수>...`이다. 요구사항에 따라서 주문 내역 형식이 일치하지 않는다면 `IllegalArgumentException` 을 발생시켜야 한다. 
주어진 형식을 정규표현식으로 바꾸기 전에 패턴을 다음과 같이 파악해볼 수 있다.
* <메뉴이름>: 3 *(메뉴 최소 글자 수)* ~ 8 *(메뉴 최대 글자 수)* 개의  **한글로 구성**
* -: 메뉴 이름과 개수 구분자
* <개수>: 숫자로 구성
* ,: 만약 주문 내역이 두 개 이상이라면 각 주문 내역 구분자

1. `[가-힣]{3,8}-[0-9]`: 정리한 주문 내역 형식을 정규표현식에 따라 정리
2. `^([가-힣]{3,8}-[0-9]+)`: **주문 내역은 반드시 하나 이상으로 시작**하고, **개수는 반드시 숫자 하나 이상** 입력
3. `^([가-힣]{3,8}-[0-9]+)(,[가-힣]{3,8}-[0-9]+)`: 두 개 이상의 주문 내역이 입력된다면, 다음 내역부터 **쉼표로 시작**
4. `^([가-힣]{3,8}-[0-9]+)(,[가-힣]{3,8}-[0-9]+)*$`: **쉼표가 붙은 주문 내역은 0 개 이상 사용**될 수 있음
<br/>
<br/>

#### 정규표현식의 편리함
처음 정규표현식을 사용할 때에는 익숙하지 않아서 해석하는데 시간이 다소 걸리고 직접 표현식을 작성하려고 하니 헷갈리는 부분이 많았다.
하지만 표현식을 통해 문자열의 형식을 간단하게 검사해볼 수 있어서 앞으로 특히 입력값의 형식을 체크할 때 유용하게 사용할 것 같다.
<br/>
<br/>
<br/>

[참고자료]<br/>
* [자바 정규식(Regular Expression) 사용법 정리 - Inpa Dev ‍](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A0%95%EA%B7%9C%EC%8B%9DRegular-Expression-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%A0%95%EB%A6%AC)
* [[JAVA] 정규표현식](https://gh402.tistory.com/54)
* [[Java] 자바 정규 표현식(Regex) 개념 정리 및 활용
출처: https://ittrue.tistory.com/177 [IT is True:티스토리]](https://ittrue.tistory.com/177)
* [[Java] String - matches : 패턴 일치 여부](https://priming.tistory.com/9)
* [[Java] 정규 표현식과 Pattern클래스 및 .matches() 메서드](https://velog.io/@kai6666/Java-%EC%A0%95%EA%B7%9C-%ED%91%9C%ED%98%84%EC%8B%9D%EA%B3%BC-Pattern%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%B0%8F-%EB%A9%94%EC%84%9C%EB%93%9C)
* [[JAVA] 자바_matches (정규 표현식과 일치하는지 여부 확인)
출처: https://mine-it-record.tistory.com/138 [나만의 기록들:티스토리]](https://mine-it-record.tistory.com/138)
* [Java - String.matches()로 문자열 패턴 확인 및 다양한 예제 소개](https://codechacha.com/ko/java-string-matches/)