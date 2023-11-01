---
title: "[우아한 테크코스-프리코스 2주차] 자동차 경주 게임 구현(리팩토링 전)"
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


**[우아한 테크코스 2주차 미션 숫자 야구 게임 GitHub Link](https://github.com/woowacourse-precourse/java-racingcar-6)**

# 1 차 미션 후
***
1 차 자동차 미션을 제출하고, 2 차 미션을 시작하기에 앞서 메일로 1 차 미션 공통 피드백이 날아왔다. 또한 2 차 미션에서는 추가된 요구사항과 지켜야할 컨벤션(commit)이 생겼다.
클린 코드, 객체 지향성을 보장한 코드, 가독성이 좋고, 깔끔한 코드.. 가 중요하다는 것은 익히 들어왔지만 이를 의식적으로 지키기 위해 노력한 코드와 그렇지 않은 코드를 직접 비교하고 싶었다.
코드 퀄리티의 변화를 비교하고자 **먼저 공통 피드백을 깊게 고려하지 않고 1차 구현한 후에, 2 차로 코드를 리팩토링** 해보기로 했다. 
<U>이번 포스팅은 리팩토링 전 코드를 리뷰한 것이다.</U>
<br/>

**[자동차 경주 게임 최종 구현(리팩토링 후) 보러가기](https://hye807n.github.io/review/woowacourse-precourse-6-java-racing-car(4).html)**

<br/>
<br/>

# 구현 과정

## 코드 구현 순서
***
### 1. view/OutputView.java, view/InputView.java

```java
package racingcar.view;

import static camp.nextstep.edu.missionutils.Console.readLine;


public class InputView {

    public static String inputCarName() {
        return readLine();
    }

    public static String inputRepeatNumber() {
        return readLine();
    }
}
```

```java
package racingcar.view;


import java.util.ArrayList;
import java.util.List;
import racingcar.model.CarModel;

public class OutputView {
    public static final String DEMAND_CAR_NAME = "경주할 자동차 이름을 입력하세요.(이름은 쉼표(,) 기준으로 구분)";
    public static final String DEMAND_REPEAT_NUMBER = "시도할 회수는 몇회인가요?";
    public static final String PRINT_RESULT = "\n실행 결과";
    public static final String PRINT_WINNER = "최종 우승자 : ";

    public static void printCarNameDemand() {
        System.out.println(DEMAND_CAR_NAME);
    }

    public static void printRepeatNumberDemand() {
        System.out.println(DEMAND_REPEAT_NUMBER);
    }

    public static void printResult(){
        System.out.println(PRINT_RESULT);
    }

    public static void printRacing(String racingState) {
        System.out.println(racingState);
    }

    public static void printWinner(List<CarModel> winnerCars) {
        StringBuilder stringBuilder = new StringBuilder();
        List<String> winners = new ArrayList<>();

        for (CarModel carModel : winnerCars) {
            winners.add(carModel.getName());
        }
        stringBuilder.append(PRINT_WINNER).append(String.join(", ", winners));
        System.out.println(stringBuilder);
    }
}
```
<br/>

* `Console.readLine()`
요구사항에 따라 `InputView.java` 에서 사용된 `amp.nextstep.edu.missionutils.Console.readLine()` 함수에 대한 분석은 [이전 포스팅](https://hye807n.github.io/review/woowacourse-precourse-6-java-baseball-3.html)에서 다루었다.
<br/>
* `printWinner(List<CarModel> winnerCars)`
우승 차량 객체들을 담은 List를 인자로 받아서 각각의 객체들의 `name`, `location` 을 출력 양식에 맞게 **`stringBuiler()`를 통해 출력**한다.
<br/>

### 2. model/CarModel.java
```java
package racingcar.model;

import java.util.List;

public class CarModel {
    private final String name;
    private int location;

    public CarModel(String carName) {
        this.name = carName;
        this.location = 0;
    }
    public String getName() {
        return name;
    }
    public void increaseLocation() {
        this.location++;
    }

    public String carLocationState() {
        StringBuilder result = new StringBuilder();

        result.append(name).append(" : ");
        result.append("-".repeat(Math.max(0, location)));
        return result.toString();
    }

    public boolean compareMaxLocation(int a) {
        return this.location >= a;
    }

}
```
<br/>

* `CarModel(String carName)`
setter를 지양하기 위해 생성자를 통해서 Car의 field 값을 지정해주었다.
<br/>

* `getname()`
car 객체를 출력하기 위해서 getter를 지양해야함을 알지만 불가피하게 사용했다.
<br/>

* `increaseLocation()`
전진 기능을 위해 추가한 함수이다.
<br/>

* `carLocationState()`
carLocation의 정보를 getter로 빼내지 않기 위해서 Carmodel 내에서 `StringBuilder()` 를 통해 객체의 정보를 빌드하여 반환하는 함수이다.
<br/>

* `compareMaxLocation(int a)`
carLocation의 정보를 getter로 접근하지 않고 우승차를 판별하기 위해 입력받은 최대 위치값보다 크다면 `true`를 반환한다.
<br/>

### 3. controller/CarController.java
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
<br/>

* `CarController(List<String> carList)`
사용자에게 입력받은 car 객체들의 이름을 하나씩 car 객체의 생성자로 넘겨주어 객체를 생성합니다. 이렇게 생성한 car 객체들을 carModelList에 담아서 **carController를 통해 car를 관리**합니다.
<br/>

* `forwardCheck()`
**기능요구사항에 따라 랜덤하게 반환된 정수값이 4 이상이라면 `true`를 반환**한다.
요구사항에 따라 `RandomNumber.java` 에서 사용된 `camp.nextstep.edu.missionutils.Randoms.randomNumberGenerator()` 함수에 대한 분석은 [이전 포스팅](https://hye807n.github.io/review/woowacourse-precourse-6-java-baseball-3.html)에서 다루었다. 
<br/>

* `carForward()`
모든 모델 객체를 한 번씩 확인하며 `forwardCheck()` 가 `true`라면 car 객체를 전진시킨다.
<br/>

* `winnerCheck(int maxLocation)`
* `maxLocationCheck(int maxLocation)`
우승자를 판별하기 위해 사용된 함수들이다. 이번 미션에서 **가장 핵심이 되는 로직**이라고 생각했다. <U>최대 위치값을 판별, 우승자 객체 판별 등을 위해 객체지향의 은닉성을 위한 규칙 중 하나인 getter, setter를 지양해야한다는 규칙을 지키기 힘들었던 로직 중 하나</U>였기 때문이다. getter를 지양하기 위해 구상한 알고리즘은 다음과 같다.
<br/>

> **객체들의 최대 위치값은 반복 횟수이다.**
> `repeatNumber` 번의 반복에서 모두 전진이 나왔다고 하면 `repeatNumber` 이 최대 위치값
>   1. 0 부터 n 까지의 위치값을 `maxLocationCheck(int maxLocation)`에 전달
>       1. `maxLocation` 이상의 위치값을 가진 자동차 객체가 있다면,
>           * 자동차 객체들로 이루어진 `winnerCars 반환`
>           * `Cars.size()`가 0보다 크므로,
>           * `winnnerCars` 우승자 리스트 초기화
>       2. `maxLocation` 이상의 위치값을 가진 자동차 객체가 없다면,
>           * 자동차 객체들로 이루어진 `winnerCars 반환`
>           * `Cars.size()`가 0보다 작으므로,
>           * `winnnerCars` 우승자값 유지
>   2. `winnerCars` 반환하여 우승차 출력

<br/>

`CarModel` 들을 관리하는 역할을 하는 `carController`를 통해 `CarModel`의 정보를 활용하는 로직을 구현하였다.
<br/>

### 4. controller/RacingController.java
```java
package racingcar.controller;

import camp.nextstep.edu.missionutils.Randoms;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import racingcar.exception.ExceptionCheck;
import racingcar.model.CarModel;
import racingcar.view.InputView;
import racingcar.view.OutputView;

public class RacingController {

    private CarController carController;
    private int repeatNumber;

    public void carNamesProcess() {
        OutputView.printCarNameDemand();
        List<String> carList = Arrays.asList(InputView.inputCarName().split(","));

        this.carController = new CarController(carList);
    }

    public void repeatNumberProcess() {
        OutputView.printRepeatNumberDemand();
        repeatNumber = Integer.parseInt(InputView.inputRepeatNumber());
        ExceptionCheck.iterateNumberValidation(repeatNumber);
    }

    public void racingStart() {
        OutputView.printResult();
        for (int i = 0; i < repeatNumber; i++) {
            carController.carForward();
        }
    }

    public void resultWinner() {
        OutputView.printWinner(carController.winnerCheck(repeatNumber));
    }
}
```
<br/>

`RacingController` 를 통해 **전반적인 게임 프로세스를 나누어서 진행**하는 함수를 생성하였다. 크게 네 가지로 나누어서 구현해보았다.
<br/>

### 5. exception/ExceptionCheck.java
```java
package racingcar.exception;

public class ExceptionCheck {
    public static void nameLengthValidation(String carName) {
        if (!(carName.length() <= 5)) {
            throw new IllegalArgumentException();
        }
    }

    public static void iterateNumberValidation(int iterateNumber) {
        if (iterateNumber <= 0) {
            throw new IllegalArgumentException();
        }
    }
}
```
<br/>

player의 입력이 잘못되었을 경우, **요구 사항인 IllegalArgumentException** 를 발생시켰다.
<br/>

* `iterateNumberValidation(int iterateNumber)`
*요구사항에 명시되어 있지는 않았지만*, 만약 반복 회수가 0 이하라면 오류를 발생시키는 코드를 자의적으로 추가해보았다.
<br/>

### 6. Application.java
```java
package racingcar;

import racingcar.controller.RacingController;
import racingcar.model.CarModel;

public class Application {
    public static void main(String[] args) {
        RacingController racingController = new RacingController();

        racingController.carNamesProcess();
        racingController.repeatNumberProcess();
        racingController.racingStart();
        racingController.resultWinner();
    }
}
```
<br/>

게임의 전반적인 진행을 위한 `racingController` 들의 함수를 실행시켰다.
<br/>
<br/>
<br/>

# 코드를 구현하는 과정에서..
***
우선 구현을 완성하는 것에만 집중하고 짜보았는데 벌써부터 수정해야 할 부분들이 조금씩 보였다.
이제 코드를 리팩토링하는 과정을 통해 퀄리티를 높여보자!