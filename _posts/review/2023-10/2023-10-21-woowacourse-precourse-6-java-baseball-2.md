---
title: "[우아한 테크코스-프리코스 1주차] 숫자야구게임 구현 과정"
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

# 구현 과정

## 코드 구현 순서
***
### 1. view/OutputView.java, view/InputView.java

```java
package baseball.view;

public class OutputView {
    public static final String START_MESSAGE = "숫자 야구 게임을 시작합니다.";
    public static final String END_MASSAGE = "3스트라이크\n3개의 숫자를 모두 맞히셨습니다! 게임 종료";
    public static final String RESTART_MESSAGE = "게임을 새로 시작하려면 1, 종료하려면 2를 입력하세요.";

    public static void printStartMessage() {
      System.out.println(START_MESSAGE);
    }

    public static void printRestartMessage() {
      System.out.println(RESTART_MESSAGE);
    }

    public static void printEndMessage() {
      System.out.println(END_MASSAGE);
    }

    public static void printNothingScore() {
      System.out.println("낫싱");
    }

    public static void printBallScore(int ballScore) {
      System.out.print(ballScore+"볼 ");
    }
    
    public static void printStrikeScore(int strikeScore) {
        System.out.println(strikeScore+"스트라이크");
    }

}
```

```java
package baseball.view;

import camp.nextstep.edu.missionutils.Console;

public class InputView {
    public static final String PRE_INPUT_MESSAGE = "숫자를 입력해주세요 : ";

    public static String inputPlayerNumber() {
        System.out.print(PRE_INPUT_MESSAGE);
        return Console.readLine();
    }

    public static String inputRestartNumber() {
        return Console.readLine();
    }
}


```

* `printBallScore(int ballScore)`
볼 점수를 출력하는 `printBallScore()` 만 `println()` 이 아닌 `print()` 를 사용했는데, 이는 출력 요구사항에서 볼 점수와 스트라이크 점수를 한 줄에 나란히 출력하기 위해 <u>줄 바꿈을 하지 않는 출력문</u>을 사용하기 위함이다.

* `Console.readLine()`
요구사항에 따라 `InputView.java` 에서 사용된 `amp.nextstep.edu.missionutils.Console.readLine()` 함수에 대한 분석은 [이전 포스팅](https://hye807n.github.io/review/woowacourse-precourse-6-java-baseball-3.html)에서 다루었다.
<br/>

> **MVC 패턴**
Model이 정보와 데이터를 담고, View가 UI를 담당한다. 
Model과 View 는 서로의 존재를 모르고 독립적이다.
Controller 가 Model-View를 중재한다. 


먼저 Controller 가 중재하기 위해 **사용할 재료(Field)값을 가지고 있는 Model**과, **사용할 기능(Method)를 가지고 있는 View**를 먼저 구현해야 하지 않을까? 라고 생각했다. 그리고 낫싱, 볼, 스트라이크를 제외한 긴 문장들은 **의도를 전달하고자 상수로 정의**해주었다. 또한 `OutputView`, `InputView` 두 객체 모두 **상태값이 없고 입출력 기능만 사용하기 위함이므로 static method로 정의**하였다.

<br/>

### 2. random/RandomNumber.java
```java
package baseball.random;

import camp.nextstep.edu.missionutils.Randoms;
import java.util.ArrayList;
import java.util.List;

public class RandomNumber {

    public static List<Integer> randomNumberGenerator() {
        List<Integer> randomNumberList = new ArrayList<>();

        while (randomNumberList.size() < 3) {
            int randomNumber = Randoms.pickNumberInRange(1, 9);
            if (!randomNumberList.contains(randomNumber)) {
                randomNumberList.add(randomNumber);
            }
        }
        return randomNumberList;
    }
}
```

<br/>

* `randomNumberGenerator()`
요구사항에 따라 `RandomNumber.java` 에서 사용된 `camp.nextstep.edu.missionutils.Randoms.randomNumberGenerator()` 함수에 대한 분석은 [이전 포스팅](https://hye807n.github.io/review/woowacourse-precourse-6-java-baseball-3.html)에서 다루었다. **`randomNumberGenerator()` 역시 `RandomNumber` class의 필드값이 없이 method 기능만 사용되므로 static으로 정의**하였다.
<br/>

View를 통해 Controller 가 사용할 기능들을 만들어주었다면, Model을 구현하기에 앞서 **Model의 랜덤한 정수 3 개로 구성된 List 형식의 Field 값을 반환해 줄 RandomNumber class를 구현**하였다.

<br/>


### 3. model/ScoreState.java
   
```java
package baseball.model;

public enum ScoreState {
    BALL_SCORE,
    STRIKE_SCORE,

}

```

`ComputerModel.java` 를 구현하다가 scoreMap의 key 값을 기존에는 `"strikeScore"`, `"ballScore"` 과 같은 string 형태로 접근했었는데, 이는 **가독성 과 싱글톤 형태를 보장하여 안정성을 높이기 위해 enum으로 변경**하였다.

<br/>

### 4. model/ComputerModel.java

```java
package baseball.model;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class ComputerModel {
    private final String computerNumber;

    public ComputerModel(List<Integer> computerNumber) {
        this.computerNumber = computerNumber.toString().replaceAll("[^0-9]","");
    }

    public Map<ScoreState, Integer> scoreCalculator(String playerNumber) {
        String computerNumber = this.computerNumber;
        Map<ScoreState, Integer> scoreMap = scoreMapInitialize(new HashMap<>());

        for (int i = 0; i < computerNumber.length(); ) {
            if (computerNumber.charAt(i) == playerNumber.charAt(i)) {
                scoreMap.put(ScoreState.STRIKE_SCORE,(scoreMap.get(ScoreState.STRIKE_SCORE) + 1));
                computerNumber = computerNumber.replace(String.valueOf(computerNumber.charAt(i)), "");
                playerNumber = playerNumber.replace(String.valueOf(playerNumber.charAt(i)), "");
                continue;
            }
            if (computerNumber.contains(Character.toString(playerNumber.charAt(i)))) {
                scoreMap.put(ScoreState.BALL_SCORE, (scoreMap.get(ScoreState.BALL_SCORE) + 1));
            }
            i++;
        }
        return scoreMap;
    }

    private Map<ScoreState, Integer> scoreMapInitialize(Map<ScoreState, Integer> scoreMap) {
        scoreMap.put(ScoreState.BALL_SCORE, 0);
        scoreMap.put(ScoreState.STRIKE_SCORE, 0);
        return scoreMap;
    }
}

```

<br/>

*  `ComputerModel(List<Integer> computerNumber)`
우선 Computer 개체가 가지고 있어야할 random한 numberList를 생성자를 통해 set해주었다. ~~(setter, getter 사용을 지양해야 하므로 생성자를 사용했다.)~~ 
<br/>


* `scoreCalculator(String playerNumber)`
핵심 로직인 점수 계산하는 로직을 구현했다. `ballScore`, `strikeScore` 점수는 `HashMap()`을 사용했다. 처음에는 그냥 `List<Integer>` 를 사용하려다가 key와 value를 통해서 접근하는 것이 가독성이 좋을 것이라고 생각했기 때문이다. 내가 구상한 알고리즘은 이렇다.  
>   1. 첫 번째 자리부터 **같은 자리에 같은 숫자가 있는 strike인지** 체크
>       1. strike 인 경우
>           * strike 점수 +1
>           * 같은 자리 같은 요소 삭제
>           * 앞 자리 숫자가 하나 삭제되었으므로 index 변수인 `i`의 값을 유지하기 위해 `i--;`
>       2. ball 인 경우
>           * ball 점수 +1
<br/>

* `scoreMapInitialize(Map<ScoreState, Integer> scoreMap)`
scoreMap을 사용하기 전에 key값과 value를 초기화하기 위해 만든 함수이다.

<br/>

### 5. exception/ExceptionCheck.java
    
```java
package baseball.exception;

import java.util.Objects;

public class ExceptionCheck {

    public static void playerInputCheck(String inputNumber) {
        if (inputNumber.length() != 3) {
            throw new IllegalArgumentException();
        }
    }

    public static void restartInputCheck(String inputNumber) {
        if (!(Objects.equals(inputNumber, "1")) && !(Objects.equals(inputNumber, "2"))) {
            throw new IllegalArgumentException();
        }
    }
}

```

* `playerInputCheck(String inputNumber)`
player 가 `computerNumber` 를 추측하기 위해 입력한 값이 3 자리가 맞는지 확인하는 함수이다. 
<br/>
* `restartInputCheck(String inputNumber)`
player 가 `computerNumber` 를 맞춘 후, 재시작/종료를 선택할 때 입력한 값이 1이나 2인지 확인하는 함수이다.
<br/>

player의 입력이 잘못되었을 경우, **요구 사항인 IllegalArgumentException** 를 발생시켰다.

<br/>

### 6. controller/StartController.java
 
```java
package baseball.controller;

import baseball.exception.ExceptionCheck;
import baseball.model.ComputerModel;
import baseball.model.ScoreState;
import baseball.random.RandomNumber;
import baseball.view.InputView;
import baseball.view.OutputView;
import java.util.Map;

public class StateController {
    private static final int GAME_STATE_RESTART = 1;

    private ComputerModel computerModel;

    public void gameStart() {
        this.computerModel = new ComputerModel(RandomNumber.randomNumberGenerator());
    }

    public boolean repeatPlayerExpect() {
        Map<ScoreState, Integer> scoreMap;
        String playerNumber = InputView.inputPlayerNumber();

        ExceptionCheck.playerInputCheck(playerNumber);
        scoreMap = computerModel.scoreCalculator(playerNumber);
        gameScoreCheck(scoreMap);
        return scoreMap.get(ScoreState.STRIKE_SCORE) == 3;
    }

    public boolean gameStateChange() {
        OutputView.printRestartMessage();

        String playerNumber = InputView.inputRestartNumber();
        ExceptionCheck.restartInputCheck(playerNumber);
        return Integer.parseInt(playerNumber) == GAME_STATE_RESTART;
    }

    private void ballStrikeScoreCheck(int strikeScore, int ballScore) {
        if (strikeScore > 0 && ballScore == 0) {
            OutputView.printStrikeScore(strikeScore);
        }
        if (strikeScore == 0 && ballScore > 0) {
            OutputView.printBallScore(ballScore);
            System.out.println("");
        }
        if (strikeScore > 0 && ballScore > 0) {
            OutputView.printBallScore(ballScore);
            OutputView.printStrikeScore(strikeScore);
        }
    }

    private void gameScoreCheck(Map<ScoreState, Integer> scoreMap) {
        int strikeScore = scoreMap.get(ScoreState.STRIKE_SCORE);
        int ballScore = scoreMap.get(ScoreState.BALL_SCORE);

        if (strikeScore == 3) {
            OutputView.printStrikeScore(strikeScore);
            OutputView.printEndMessage();
        }
        if (strikeScore == 0 && ballScore == 0) {
            OutputView.printNothingScore();
        }
        if ((strikeScore > 0 || ballScore > 0) && strikeScore != 3) {
            ballStrikeScoreCheck(strikeScore, ballScore);
        }
    }
}

```
<br/>

* `gameStart()`
새 게임이 시작될 때마다 실행되는 함수로, `computerModel` 의 임의의 숫자 3 개를 초기화 하기 위한 함수이다.
<br/>

* `repeatPlayerExpect()`
player가 computer의 숫자를 추측할 때마다 실행되는 함수로, player의 입력을 받아서 자리 수가 3 개인지 확인을 하고 점수를 계산한다. 그리고 점수를 출력하고 3 스트라이크라면 `true`를 반환한다.
<br/>

* `gameStateChange()`
player가 computer의 숫자를 맞춘 후에 재시작 문구를 출력하고 재시작/종료 중 선택하여 입력한 값이 1과 2인지 확인한다. 그리고 입력한 값에 따라 재시작이면 `true`를 반환한다.
<br/>

* `ballStrikeScoreCheck(int strikeScore, int ballScore)`
player가 추측한 값의 결과가 낫싱/3 스트라이크 가 아닌 경우에 볼과 스트라이크 점수를 구분하여 출력하기 위한 함수이다.
<br/>

* `gameScoreCheck(Map<ScoreState, Integer> scoreMap)`
매개변수인 `scoreMap`에서 점수를 확인하여 3 스트라이크라면 점수와 함께 종료 메세지를, 낫싱이라면 낫싱을, 그 외의 경우에는 `ballStrikeScoreCheck()` 를 호출하여 점수를 출력한다.
<br/>

Controller는 Model과 View와는 다르게 두 개체의 정보를 알 수 있으므로 ComputerModel을 field값으로 가지고 있다. 게임 재시작/종료에 대한 입력값을 구분하기 위하여 재시작 입력값을 상수로 선언하였다.

<br/>

### 7. Application.java

```java
package baseball;

import baseball.controller.StateController;
import baseball.view.OutputView;

public class Application {
    public static void main(String[] args) {
        StateController controller = new StateController();
        boolean gameState = true;
        boolean restart = false;

        OutputView.printStartMessage();
        controller.gameStart();

        while (gameState) {
            if(controller.repeatPlayerExpect()) {
                restart = controller.gameStateChange();
                gameState = restart;
            }
            if (restart) {
                controller.gameStart();
                restart = false;
            }
        }
    }
}

```
<br/>

controller 객체를 생성하여 게임 시작을 제어한다. 또한 `gameState` 를 true로 초기화하여 현재 게임 진행 상태를 구별하고 `restart` 를 통해 player 가 정답을 맞춘 후에 재시작/종료 선택지 중 입력한 값을 구분한다.
만약 종료한다면 현재 게임 진행 상태와 재시작 여부를 false로 바꾸고, 재시작한다면 게임 진행 상태를 바꾸지 않은 채(true) `computerNumber` 를 초기화하는 함수를 실행한다.

<br/>
<br/>
<br/>

# 코드를 구현하는 과정에서..
### Enum
C와 C# 에서만 사용해보아서 java 에서 enum은 존재 여부만 알고있던 채로 처음 사용해보았는데 크게 다를 것이 없어서 어렵지는 않았다.
### 객체지향 - 상속성, 추상화
항상 가독성과 재사용성을 유념하여  method를 최소한의 역할만 부여하며 작은 단위로 쪼개서 구현하였다. 
### 객체지향 - 은닉성
그리고 객체 지향의 특징인 은닉화를 지키기 위해 되도록 접근제한자를 private으로 선언했으며, 식별자를 역할에 맞게 선언하기 위해 노력했다.
### 상수 사용
가독성과 속도 측면에서 원시 데이터를 사용하는 것을 지양하면서 되도록 상수로 선언하여서 사용하였다.
### commite message

<details>
<summary>commit log</summary>
<div>
<img src="https://github.com/hYe807n/hYe807n.github.io/assets/79916066/d95bc439-1a1a-4f83-befe-489f24a6cedc" />
</div>

</details>
<br/>

commit message 를 명확하고 깔끔하게 남기기 위해 `fix:`, `feat:` 등의 상황에 맞는 양식을 찾아보고, 구**현 기능 목록을 하나씩 구현할 때마다 commit**을 남겼다. **push는 하나의 class의 구현이 얼추 끝났을 때마다 실행했**다. 처음에는 익숙하지 않았지만 시간이 지날수록 능숙해지고 진행 사항을 확인하기에도 깔끔해서 보기 좋다는 것을 깨닫고, 미션 진행 과정 뿐만 아니라 블로그 포스팅과 같이 혼자 작업을 할 때에도 commit 양식을 사용하는 것을 지향하게 되었다. 또한 목록을 하나씩 체크해나갈 때마다 느껴지는 성취감도 있었다.

<br/>
<br/>
<br/>

[참고자료]<br/>

* [[JAVA]자바 문자열 비교 equals(), == 사용법 및 차이점](https://lnsideout.tistory.com/entry/JAVA%EC%9E%90%EB%B0%94-%EB%AC%B8%EC%9E%90%EC%97%B4-%EB%B9%84%EA%B5%90-equals-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%B0%A8%EC%9D%B4%EC%A0%90)
* [Java의 문자열에서 특정 문자 제거](https://www.techiedelight.com/ko/remove-certain-characters-from-string-java/)
* [How can I convert a char to CharSequence? [duplicate]](https://stackoverflow.com/questions/56056419/how-can-i-convert-a-char-to-charsequence)
* [[Java] try-catch 문의 주의사항 (feat. try-with-resource)](https://parkadd.tistory.com/66)``
* [[Java] Enum을 사용하는 이유와 사용법](https://velog.io/@mooh2jj/Java-Enum%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
* [OOP (Object Oriented Programming: 객체지향개발)](https://beerntv.wordpress.com/2017/01/18/0118-%E1%84%8E%E1%85%AE%E1%84%89%E1%85%A1%E1%86%BC%E1%84%92%E1%85%AA-%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%89%E1%85%B2%E1%86%AF%E1%84%92%E1%85%AA-%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%82%E1%85%B5%E1%86%A8/)