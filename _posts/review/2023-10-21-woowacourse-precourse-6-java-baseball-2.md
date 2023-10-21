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
요구사항에 따라 InputView.java 에서 사용된 `amp.nextstep.edu.missionutils.Console.readLine()` 함수에 대한 분석은 [이전 포스팅](https://hye807n.github.io/review/woowacourse-precourse-6-java-baseball-3.html)에서 다루었다.
</br>

### 2. Random