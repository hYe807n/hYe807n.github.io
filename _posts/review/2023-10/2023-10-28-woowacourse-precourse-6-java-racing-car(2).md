---
title: "[우아한 테크코스-프리코스 2주차] 일급 컬렉션 적용하기"
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

# 일급 컬렉션?
***
우아한 테크코스의 프리코스 2 주차 미션을 진행하면서 **CarModel 객체를 관리하는 CarController 를 생성하였는데 이는 View 와 Model 을 중재하는 Controller의 역할보다 Car 객체들의 리스트를 갖는 일급 컬렉션**에 더 가깝다는 판단을 했다.
<U>그렇다면 일급 컬렉션이란 무엇일까?</U>

일급 컬렉션(First-Class Collection)이라는 용어는 [소트웍스 앤솔로지](https://coupa.ng/bgRZDf)의 객체지향 생활체조 파트에서 처음 언급되었다. Java에서 일급 컬렉션은 다른 객체와 동등한 지위를 갖는 Collection 객체를 말하는데, **Collection을 Wrapping 하면서 외에 다른 field가 없는 상태**를 말한다. 포비님께서 TDD, 클린코드를 지향하기 위해서 일급 컬렉션 사용하는 것을 추천하셨다. 
<br/>
<br/>

# 일급 컬렉션 규칙
***
> **규칙 8: 일급 콜렉션 사용**
> 이 규칙의 적용은 간단하다.
> 콜렉션을 포함한 클래스는 반드시 다른 멤버 변수가 없어야 한다.
> 각 콜렉션은 그 자체로 포장돼 있으므로 이제 콜렉션과 관련된 동작은 근거지가 마련된셈이다.
> 필터가 이 새 클래스의 일부가 됨을 알 수 있다.
> 필터는 또한 스스로 함수 객체가 될 수 있다.
> 또한 새 클래스는 두 그룹을 같이 묶는다든가 그룹의 각 원소에 규칙을 적용하는 등의 동작을 처리할 수 있다.
> 이는 인스턴스 변수에 대한 규칙의 확실한 확장이지만 그 자체를 위해서도 중요하다.
> 콜렉션은 실로 매우 유용한 원시 타입이다.
> 많은 동작이 있지만 후임 프로그래머나 유지보수 담당자에 의미적 의도나 단초는 거의 없다.
<br/>
<br/>

# 일급 컬렉션 적용 비교

## 적용 전
```java
package racingcar.controller;

import static racingcar.exception.ExceptionCheck.nameLengthValidation;

import camp.nextstep.edu.missionutils.Randoms;
import java.util.ArrayList;
import java.util.List;
import racingcar.exception.ExceptionCheck;
import racingcar.model.CarModel;
import racingcar.view.OutputView;

public class CarController {

    private List<CarModel> carModelList = new ArrayList<>();

    public CarController(List<String> carList) {
        for (String s : carList) {
            ExceptionCheck.nameLengthValidation(s);
            this.carModelList.add(new CarModel(s));
        }
    }

    public void carForward() {
        for ( CarModel carModel : carModelList) {
            if (forwardCheck()) {
                carModel.increaseLocation();
            }
            OutputView.printRacing(carModel.carLocationState());
        }
        System.out.println("");
    }

    public boolean forwardCheck() {
        return Randoms.pickNumberInRange(0, 9) >= 4;
    }

    public List<CarModel> winnerCheck(int maxLocation) {
        List<CarModel> winnerCars = new ArrayList<>();
        List<CarModel> Cars;

        for (int i = 0 ;i <= maxLocation; i++) {
            Cars = maxLocationCheck(i);
            if (Cars.size() > 0) {
                winnerCars = Cars;
            }
        }
        return winnerCars;
    }

    public List<CarModel> maxLocationCheck(int maxLocation) {
        List<CarModel> winnerCars = new ArrayList<>();

        for ( CarModel carModel : carModelList) {
            if (carModel.compareMaxLocation(maxLocation)) {
                winnerCars.add(carModel);
            }
        }
        return winnerCars;
    }
}
```

## 적용 후
```java
package racingcar.model;

import camp.nextstep.edu.missionutils.Randoms;
import java.util.List;
import racingcar.view.OutputView;

public class Cars {

    public static final int MAX_NUMBER = 9;
    public static final int MIN_NUMBER = 0;
    private final List<Car> carList;

    public Cars(List<Car> carList) {
        this.carList = carList;
    }

    public void moveCars() {
        for (Car car : carList) {
            car.forward(Randoms.pickNumberInRange(MIN_NUMBER, MAX_NUMBER));
            OutputView.printRacing(car.getName(), car.getLocation());
        }
        System.out.println("");
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
}
```

<br/>

우선 변경 전의 코드는 **Car 객체들을 CarsController 들을 통해 관리**한다. 처음 구현할 때에는 일급 컬렉션의 필요성을 알지 못한 채로 Car 객체들을 관리하니 Controller 로 해야하나? 라고 생각했던 것 같다.
하지만 <U>이는 View와 Model을 중재하는 역할을 하는 Controller의 원래 역할과도 맞지 않을 뿐더러 이러한 역할을 하는 Car의 일급 컬렉션의 존재를 간과한 것</U>이였다. *(이름만 Controller 였지 사실상 일급 컬렉션의 역할을 수행했던 것 같다.)*
그럼 일급 컬렉션을 사용함으로서 어떤 이점을 얻을 수 있을까?
<br/>
<br/>

# 일급 컬렉션 이점
***
> 1. 비즈니스에 종속적인 자료구조
> 2. Collection의 불변성을 보장 **(할 수도 있음)**
> 3. 상태와 행위를 한 곳에서 관리
> 4. 이름이 있는 컬렉션
<br/>

## 1. 비즈니스에 종속적인 자료구조
이는 구현하고자 하는 **비즈니스에 맞게 특정 조건을 일급 컬렉션을 통해 구현하여 새로운 자료구조를 생성할 수 있다는 뜻**이다.

```java
Car car1 = new Car("pobi");
Car car2 = new Car("yeon");
Car car3 = new Car("jjang");

List<Car> carList = new List<Car>(car1, car2, car3);
carSizeValidation(carList);
carNameValidation(carList);
```
<br/>

예를 들어 **Car 객체는 5 개 밖에 생성할 수 없고,** **Car객체의 이름은 중복될 수 없다** 는 조건이 추가되었다고 해보자. 그럼 `List<Car> carList` 가 사용되는 모든 곳에서 `validation` 을 검증했어야됐을 것이다. 
<br/>

```java
public class Cars {

    private List<Car> carList;

    public Cars(List<Car> carList) {
        carSizeValidation(carList);
        carNameValidation(carList);
        this.carList = carList;
    }
    
    ...
 
}
```
<br/>

하지만 이는 일급 컬렉션을 통해 `carList` 가 생성될 때마다 생성자로 검증하여 다음과 같이 해결할 수 있다.
<br/>
<br/>

## 2. Collection의 불변성을 보장 **(할 수도 있음)**
공부하면서 굉장히 헷갈렸던 것인데, 많은 블로그에서는 Collection이 불변성을 보장한다고 적혀있다. 하지만 **`Object Calisthenics`의 내용 일부를 참고해보면, `"일급컬렉션은 불변으로 만들어야 한다.”`, `“일급컬렉션의 이점은 불변이다.”`는 내용을 언급하고 있지 않다.**  
<U>불변으로 만들 수 있지만, 불변을 보장하지는 않는다.</U> 왜 그런 것일까? 그럼 불변으로 만들기 위해 어떻게 해야할까?
<br/>

위에 첨부했던 일급 컬렉션을 적용하여 수정한 Cars 를 보면, `private final List<Car> carList;` 로 리스트가 선언되어있다. 그런데 그 밑에 **`moveCars()` 를 통해 일부 객체의 상태값을 바꿀 수 있다는 것을 알 수 있다.**  
이는 Java의 **<U>`final` 키워드가 정확히 불변을 만드는 것이 아니라 변수에 재할당만 금지</U>**하기 때문이다.  
만약 불변을 보장하고 싶다면, **일급 컬렉션 객체가 wrapping 하고 있는 객체의 값을 변경할 수 있는 메소드가 없는 Collection을 만들면 불변을 보장**할 수 있게 된다.  
때문에 불변성을 보장하지도 않고 할 필요도 없지만, 불변으로 만들 수는 있는 것이다.  
<br/>
<br/>

## 3. 상태와 행위를 한 곳에서 관리
이는 일급 컬렉션으로 객체를 한 곳에서 관리함으로서 각 객체마다 **중복된 메서드의 생성하는 것을 해결**할 수 있다.

예를 들어 `BigCar` 객체와 `SmallCar` 객체 내에서 전진하는 기능 각각 있다면, 이를 `Car` 일급 컬렉션을 통해 관리하여 중복된 메서드를 하나로 생성할 수 있는 것이다.
각각의 **객체의 field 값과 method를 한 번에 관리하기 때문에 외부에서 각각 변경할 수 없이 한 곳에서 모든 객체를 관리**하게 된다.
<br/>
<br/>

## 4. 이름이 있는 컬렉션
컬렉션에 이름을 붙일 수 있다는 것이 마지막 장점이다.
```java
List<Car> bigCar = new ArrayList<>();
List<Car> smallCar = new ArrayList<>();
```
<br/>

예를 들어 아까와 같이 `bigCar` 과 `smallCar` 객체 list를 관리하기 위한 코드를 작성했다. 하지만 **변수명을 통해 두 list를 구분을 지으면, 검색하는 것에 한계가 있고 사람마다 식별자 짓는 방법은 다르기 때문에 협업 시 검색하기 어렵다는 점**이 있다. 또한 **변수명에 의미를 크게 부여하기 어려우므로 명확한 표현이 어려우므로 소통에 어려움**이 발생할 수 있다.
<br/>

```java
BigCar bigCar = new BigCar();
SmallCar smallCar = new SmallCar();
```
<br/>

다음과 같이 컬렉션으로 관리하게 되면 해당 **컬렉션 기반으로 검색을 함으로서 해결**할 수 있다. 또한 **표현 역시 컬렉션에 맞춰서 사용함**으로서 소통이 원활해질 수 있다는 이점이 있다.

<br/>
<br/>
<br/>

[참고자료]<br/>
* [일급 컬렉션 (First Class Collection)의 소개와 써야할 이유](https://jojoldu.tistory.com/412)
* [일급 컬렉션이란?](https://velog.io/@seongwon97/%EC%9D%BC%EA%B8%89-%EC%BB%AC%EB%A0%89%EC%85%98%EC%9D%B4%EB%9E%80)
* [일급 컬렉션(First-Class Collection)이란?](https://dkswnkk.tistory.com/696)
* [일급 컬렉션 (First Class Collection) 을 사용하자](https://dev-cool.tistory.com/28)
* [일급 컬렉션을 사용하는 이유](https://tecoble.techcourse.co.kr/post/2020-05-08-First-Class-Collection/)
* [[Java] First Class Collection(일급 컬렉션)](https://jackjeong.tistory.com/entry/Java-First-Class-Collection%EC%9D%BC%EA%B8%89-%EC%BB%AC%EB%A0%89%EC%85%98)
* [First Class Collection(일급 컬렉션)](https://jandari91.github.io/posts/FirstClassCollection/)