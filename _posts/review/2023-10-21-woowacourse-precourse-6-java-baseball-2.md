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
<details>
    <summary>OutputView.java</summary>

```java
    package baseball.view;

    public class OutputView {
        public static final String START_MESSAGE = "숫자 야구 게임을 시작합니다.";
        public static final String END_MASSAGE = "3스트라이크\n3개의 숫자를 모두 맞히셨습니다! 게임 종료";
        public static final String RESTART_MESSAGE = "게임을 새로 시작하려면 1, 종료하려면 2를 입력하세요.";

        public void printStartMessage() {
        System.out.println(START_MESSAGE);
        }

        public void printRestartMessage() {
        System.out.println(RESTART_MESSAGE);
        }

        public void printEndMessage() {
        System.out.println(END_MASSAGE);
        }

        public void printNothingScore() {
        System.out.println("낫싱");
        }

        public void printBallStrikeScore(int ballScore, int strikeScore) {
        System.out.println(ballScore+"볼 "+strikeScore+"스트라이크");
        }

    }
```
</details>
<details>
    <summary>InputView.java</summary>

```java
package baseball.view;

import camp.nextstep.edu.missionutils.Console;

public class InputView {
    public static final String PRE_INPUT_MESSAGE = "숫자를 입력해주세요 : ";

    public String inputPlayerNumber() {
        System.out.println(PRE_INPUT_MESSAGE);
        return Console.readLine();
    }

    public String inputRestartNumber() {
        return Console.readLine();
    }
}

```
</details>
</br>


> **MVC 패턴**
Model이 정보와 데이터를 담고, View가 UI를 담당한다. 
Model과 View 는 서로의 존재를 모르고 독립적이다.
Controller 가 Model-View를 중재한다. 

먼저 Controller 가 중재하기 위해 **사용할 재료(Field)값을 가지고 있는 Model**과, **사용할 기능(Method)를 가지고 있는 View**를 먼저 구현해야 하지 않을까? 라고 생각했다. 그리고 낫싱, 볼, 스트라이크를 제외한 긴 문장들은 **의도를 전달하고자 상수로 정의**해주었다.
요구사항에 따라 `InputView.java` 에서 사용된 `amp.nextstep.edu.missionutils.Console.readLine()` 함수에 대한 분석은 [이전 포스팅](https://hye807n.github.io/review/woowacourse-precourse-6-java-baseball-3.html)에서 다루었다.

<br/>

### 2. random/RandomNumber.java
<details>
    <summary>RandomNumber.java</summary>
    
```java
package baseball.Random;

import camp.nextstep.edu.missionutils.Randoms;
import java.util.ArrayList;
import java.util.List;

public class RandomNumber {

    public List<Integer> randomNumberGenerator() {
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
</details>


View를 통해 Controller 가 사용할 기능들을 만들어주었다면, Model을 구현하기에 앞서 **Model의 랜덤한 정수 3 개로 구성된 List 형식의 Field 값을 반환해 줄 RandomNumber class를 구현**하였다.

<br/>

### 3. model/ComputerModel.java
<details>
    <summary>ComputerModel.java</summary>
    
```java
package baseball.model;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class ComputerModel {
    private String computerNumber;

    public ComputerModel(List<Integer> computerNumber) {
        this.computerNumber = computerNumber.toString();
    }

    public Map<String, Integer> scoreCalculator(String playerNumber) {
        Map<String, Integer> scoreCounter = new HashMap<String, Integer>();
        String computerNumber = this.computerNumber;
        scoreCounter.put("ballScore", 0);
        scoreCounter.put("strikeScore", 0);

        for (int i = 0; i < computerNumber.length(); i++) {
            if (computerNumber.charAt(i) == playerNumber.charAt(i)) {
                String sameNumber = String.valueOf(computerNumber.charAt(i));
                scoreCounter.put("strikeScore",(scoreCounter.get("strikeScore") + 1));
                computerNumber = computerNumber.replace(sameNumber, "");
                playerNumber = playerNumber.replace(sameNumber, "");
                i--;
            }
            if (computerNumber.contains(Character.toString(playerNumber.charAt(i)))) {
                scoreCounter.put("ballScore",(scoreCounter.get("ballScore") + 1));
            }
        }
        return scoreCounter;
    }
}


```
</details>

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
<br/>
<br/>

[참고자료]<br/>

* [[JAVA]자바 문자열 비교 equals(), == 사용법 및 차이점](https://lnsideout.tistory.com/entry/JAVA%EC%9E%90%EB%B0%94-%EB%AC%B8%EC%9E%90%EC%97%B4-%EB%B9%84%EA%B5%90-equals-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%B0%A8%EC%9D%B4%EC%A0%90)
* [Java의 문자열에서 특정 문자 제거](https://www.techiedelight.com/ko/remove-certain-characters-from-string-java/)
* [How can I convert a char to CharSequence? [duplicate]](https://stackoverflow.com/questions/56056419/how-can-i-convert-a-char-to-charsequence)