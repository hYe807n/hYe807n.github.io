---
title: "[우아한 테크코스-프리코스 2주차] Collections, Stream 적용하기"
layout: post
subtitle: "woowacourse precourse 6 - 2nd week mission"
category: review devlog
tags: java woowacourse-precourse-6
published: true
future: true
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}



**[우아한 테크코스 2주차 미션 숫자 야구 게임 GitHub Link](https://github.com/woowacourse-precourse/java-racingcar-6)**

# Collections, Stream()
***
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/cd0a831a-7266-486f-9ee8-334cb958fa5f)
<br/>

지난 1 주차 미션 공통 피드백 사항으로 직접 로직을 구현하기 보다 JAVA 에서 제공하는 API와 Java Collection을 적극적으로 활용하라는 내용이 첨부되어있었다. 그래서 이를 활용하기 위해 미션 해결 과정에서 필요한 Collection과 Stream에 대해 알아보았다.
<br/>
<br/>

# controller/RacingController.java
```java
    private void inputNames() {
        OutputView.printCarName();
        List<Car> carList = Arrays.stream(InputView.read().split(","))
            .map(Car::new)
            .collect(Collectors.toList());

        this.cars = new Cars(carList);
    }
```
<br/>

* ## `Arrays.stream` 배열 스트림
스트림은 배열 또는 컬렉션 인스턴스를 이용해서 사용할 수 있다.
> 입력받은 연속된 자동차들의 이름을 `","` 를 기준으로 나누어서 배열을 생성하기 위해  사용

<br/>

* ## Mapping
`map` 은 요소들을 하나씩 특정 값으로 변환해준다. <U>stream에 들어가있는 값이 input, 특정 로직을 거친 output이 새로운 stream에 다시 담기는 것을 mapping</U>이라고 한다.
> 이름값 리스트를 Car 객체로 mapping 해주기 위해 사용

<br/>

* ## `Collectors.toList()`
stream을 통해 작업한 결과를 다시 list로 반환한다.
> 생성된 Car 객체들을 다시 list에 담기 위해 사용

<br/>
<br/>

# model/Cars.java
```java
public void moveCars() {
        carList.forEach(car -> {
            car.forward(Randoms.pickNumberInRange(MIN_NUMBER, MAX_NUMBER));
            OutputView.printRacing(car.getName(), car.getLocation());
        });
        System.out.println();
    }

public List<Car> winner() {
    int maxLocation = maxLocation();

    return carList.stream()
        .filter(car -> car.isMaxLocation(maxLocation))
        .toList();
}

private int maxLocation() {
    return carList.stream()
        .mapToInt(Car::getLocation)
        .max()
        .getAsInt();
}
```
<br/>

* ## Iterating
`forEach()` 는 요소를 순환하며 반복하는 작업을 수행한다.
> carList의 `car.forward()` 를 수행하기 위해 사용

<br/>

* ## Filtering
`filter()` 는 stream 요소들을 하나씩 선발하여 걸러내는 작업을 한다.
> 최대 위치 값과 같은 car 객체만 filtering 하기 위해 사용

<br/>

* ## Calculating
`max()` 는 stream 요소들 중 최대값을 선별한다.
> Car 객체들의 location 값 중 최대값을 계산하기 위해 사용

<br/>

* ## `getAsInt()`
값을 반환하고, 없다면 예외를 던진다.
> Car 객체들의 location 값 중 최대값을 반환하는 과정에서 예외 상황을 처리하기 위해 사용

<br/>
<br/>
<br/>

[참고자료]<br/>
* [자바 Collection에서 특정 값이 가장 큰 element를 가져오는 방법](https://www.slipp.net/questions/552)
* [Java 스트림 Stream (1) 총정리](https://www.slipp.net/questions/552)
* [[Java] 람다식에서 메서드 참조/생성자 참조 사용법](https://mong9data.tistory.com/130)
* [Java의 List를 상황에 맞게 생성해보자 ( asList(), emptyList(), singletonList() )](https://alwayspr.tistory.com/28)