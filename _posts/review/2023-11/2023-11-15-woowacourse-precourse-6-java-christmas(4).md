---
title: "[우아한 테크코스-프리코스 4주차] 크리스마스 프로모션 미션 구현 과정"
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

# 과제 진행 요구 사항 - 클래스 분리
***
3 주차에 이어서 4 주차 과제도 클래스 분리에 포인트를 맞춰서 과제를 진행해보았다. 지금까지 공부했던 부분들을 다시 상기하며 간단히 정리해보았다.

> * 객체 역할에 맞는 기능과 책임 부여
> * 컨벤션을 숙지하여 클린 코드/TDD 지향
> * 함수의 기능은 최소한(15줄 이내)으로 구현
> * 원시 데이터보다 `ENUM` 혹은 상수 사용 지양
> * 배열보다 Collections 사용
> * Collections를 통해 사용할 수 있는 stream, java API 적극 활용
>   * 단, 단순 for문은 stream할 시 성능 저하 고려
> * 같은 객체를 여러 개 사용할 경우 일급컬렉션 적용 고려
> * 은닉성을 고려하여 가능한 `private`으로 선언
> * 상태값이 없는 객체라면 static으로 선언하여 메모리 절약
> * 단위테스트가 용이하도록 전역 변수보다 매개 변수 활용
> * 객체의 값을 가져오는 것이 아닌, 객체에게 물어보는 형태로 구현

.<br/>
.<br/>
.<br/>

등등 더 적으면 굉장히 많지만 적다보면 정말 지금까지 적은 내용을 모두 다 가져와야할 것 같아서 이 정도만 적어보았고, 코드를 작성하면서 특히 유념했던 부분들이다.
<br/>
<br/>

# 구현 코드
***
### 클래스 분리
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/e7fb41a3-de55-4178-92a9-f45af6b79236)

> * model
>   * Date: 방문 날짜 관련 로직
>   * Event: 이벤트 구분, 가격 연산 관련 로직
>   * Order: 단일 주문 관련 로직
>   * Orders: 전체 주문 처리 관련 로직
> * enums
>   * Badges: 배지 종류와 등급 기준 집합
>   * Days: 요일과 요일 구분을 위한 숫자값 집합
>   * Event: 이벤트 이름, 할인 금액 등을 위한 데이터 집합
>   * Menus: 메뉴 이름과 가격 등의 데이터 집합
>   * Messages: 출력 양식을 관리하기 위한 집합
> * controller
>   * OrderController: 주문 관련 로직과 입출력 과정을 중재하기 위한 컨트롤러
> * view 
>   * InputView: 입력 관련 view class
>   * OutputView: 출력 관련 view class
> * /test: 단위 테스트, 인수 테스트


> 구체적인 함수 선언부와 설명 **부정 행위로 간주되는 것을 방지하기 위해 일부만 공개하고, 추후에 포스팅**할 예정이다. 몇 가지 기본적인 함수 형태와 간략한 설명만 첨부했다.
## model
***
### Date
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/b293d91c-c21a-4d9e-a96c-b44eaf44eef1)

* `publid Date(String number)`: Date 생성자로, String 형태로 입력받은 number를 field 값으로 선언하기 전에 validation을 진행한다.
    * `number.matches()`: 입력받은 숫자의 형식을 검증한다. 인자로 사용된 정규표현식은 [이전 포스팅을 참고](https://hye807n.github.io/review/woowacourse-precourse-6-java-christmas.html)하면 도움이 될 것 같다.
* `private void validate(String number)`: 입력받은 number 가 *1)숫자로만 구성되어 있는지*, *2) 1 이상 31 이하* 인지 확인하는 검증을 수행한다.

<br/>
<br/>

### Event

* `String calculateWeekday(int date)`: 인자로 받은 날짜가 금요일/토요일일지 확인하여 주말 혹은 평일을 반환한다.
* `boolean isSpecialDay(int date)`: 인자로 받은 날짜가 미션에서 지정한 `"특별할인"`에 해당하는 날짜(일요일 혹은 크리스마스 당일)에 해당하는 여부를 반환한다.
* `Map<String, Integer> calculateEventDiscount(int date, int menuCount)`: 적용된 이벤트 목록 중에서도 총혜택금액에 합산될 항목들을 정리하여 `map` 형태로 반환한다.
* `int calculateDDayDiscount(int date)`: 미션에서 지정한 `디데이힐인` 금액을 날짜를 통해 계산하여 반환한다.
* `int christmasEventPrice(int menuCount)`: 인자로 받은 평일/주말 할인에 해당하는 메뉴 개수를 받아서 할인 금액을 반환한다.
* `String checkBadge(int totalDiscount)`: 인자로 받은 할인 금액에 따라 배지 등급 enum 을 반환한다.
<br/>

> Event class 내부의 함수들은 Event 항목들의 해당 여부, Event 할인 액수 연산 관련 로직을 수행한다.
> 연산하기 위해 필요한 값인 날짜, 금액 값들은 인자로 받고 class 의 상태값은 없어도 된다.
> 그러므로 **Event 내부의 함수들 역시 static으로 사용되어도 무방**하므로 static으로 선언해주었다.

<br/>
<br/>

### Order
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/09f6d662-cfff-4bd7-a455-4392f2bd8ca5)

* `publid Order(String name, int count)`: 인자로 받은 메뉴의 이름을 `enum Menu` 객체로 필드값에 담는다.

<br/>
<br/>

### Orders
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/0c51c53a-ebb4-42e1-b1ac-ad9fcd32b89c)
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/64e202eb-c422-43b8-a498-baa9c5e5bcf5)

* `public Orders(String orders)`: 입력받은 주문 내역들의 형식을 확인하고, 각각 메뉴 이름과 개수를 검증한 후에 order 들의 객체를 담은 orders 리스트를 선언한다.
* `Map<Menus, Integer> menus()`: 주문 내역을 출력하기 위해 메뉴 이름, 개수를 담은 객체, 개수의 총합을 반환한다.
* `int totalPrice()`: 총 주문금액을 연산하여 반환한다.
* `int countMenuCategory(String eventType)`: 인자로 받은 특정 카테고리 메뉴 개수를 반환한다.
* `void validateForm(String orders)`: 입력받은 주문 내역의 형식을 검증한다. 사용된 정규표현식은 [이전 포스팅을 참고](https://hye807n.github.io/review/woowacourse-precourse-6-java-christmas.html)하면 도움이 될 것 같다.
* `List<String> parseName(String orders)`: 인자로 받은 전체 주문 내역에서 메뉴 이름만을 담은 `List`를 반환한다.
* `List<String> parseCount(String orders)`: 인자로 받은 전체 주문 내역에서 메뉴 개수만을 담은 `List`를 반환한다.
* `void validateArgument(List<String> orderName, List<Integer> orderCount)`: 요구사항에 명시된 *1) 메뉴이름이 중복되지 않을 것, 2) 음료만 주문 불가능, 3) 주문한 메뉴 개수는 20 개 이하일 것, 4) 존재하지 않은 메뉴 입력받지 않을 것, 5) 주문한 메뉴 개수는 1 개 이상일 것* 에 대한 예외 처리 한 번에 하는 함수이다.
    * `boolean isDuplicated(List<String> orders)`: 메뉴 이름이 중복에 중복 시 예외 발생
    * `boolean isNotOnlyDrinks(List<String> orders)`: 주문 메뉴 카테고리가 음료만 있을 시 예외 발생
    * `boolean isNonexistentMenu(List<String> orders)`: 메뉴 이름이 Menus enum 에 존재하지 않을 시 예외 발생
    * `boolean isOverMenuCount(List<Integer> orders)`: 주문 메뉴 개수의 총합이 20 초과 시 오류 발생
    * `boolean isLessCount(List<Integer> orders)`: 주문 메뉴의 개수가 하나라도 1 미만일 시 오류 발생
<br/>
<br/>

## enums
***
#### Badges, Days, Event, Menus, Messages
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/c8578d1a-9f5e-4615-a58c-4e1ddb44565b)
각각의 enum 들은 연관된 값끼리 속성값으로 묶어주었다. 그 중에서도 `Menus` enum 내부에 인자로 받은 `String` 값이 메뉴 항목에 있는 것인지 `booleam 값으로 반환` 하는 함수를 구현했다.
이를 활용하여 사용자가 입력한 메뉴 명이 있는 것인지 `true/false` 값으로 검증한다.

## controller
***
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/07c742be-d7b4-49dc-a47d-3b43b1f65214)
`controller` 를 통해 `model`이 연산한 값을 `view`의 인자로 넘겨주어 출력 형식에 맞게 주어진 값을 연산한 결과를 보이도록 중재한다. 

> 단위테스트 메서드 선언부 역시 **부정 행위로 간주되는 것을 방지하기 위해 추후에 포스팅**할 예정이다.

<br/>
<br/>
<br/>

[참고자료]<br/>
* [[우아한 테크코스-프리코스 4주차] String.matches() 사용해보기](https://hye807n.github.io/review/woowacourse-precourse-6-java-christmas.html)
* [[우아한 테크코스-프리코스 4주차] ERROR 해결 @ParameterizedTest ParameterResolutionException](https://hye807n.github.io/review/woowacourse-precourse-6-java-christmas(2).html)
* [[우아한 테크코스-프리코스 3주차] 단위테스트 코드 리팩토링](https://hye807n.github.io/review/woowacourse-precourse-6-java-christmas(3).html)