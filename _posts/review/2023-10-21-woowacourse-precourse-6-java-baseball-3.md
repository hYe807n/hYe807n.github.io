---
title: "[우아한 테크코스-프리코스 1주차] 숫자야구게임 API 내부 리뷰"
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

# 프로그래밍 요구 사항 - 제공 API 사용
![프로그래밍 요구사항](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/94b38136-c470-4f45-bff8-e1481c8eda6c)

<br/>

프로그래밍 요구 사항에 사용해야하는 API 두 가지가 있다. 코드 내에서 `Ctrl+B`를 누르면 내부 함수로 타고 들어갈 수 있다.
이제 이 API를 활용하기 위해 내부 코드를 탐색해보자!

<br/>
<br/>

## 1. camp.nextstep.edu.missionutils.Console
### readLine()
```java
package camp.nextstep.edu.missionutils;

import java.util.Scanner;

public class Console {
    private static Scanner scanner;

    private Console() {
    }

    public static String readLine() {
        return getInstance().nextLine();
    }

    public static void close() {
        if (scanner != null) {
            scanner.close();
            scanner = null;
        }
    }

    private static Scanner getInstance() {
        if (scanner == null) {
            scanner = new Scanner(System.in);
        }
        return scanner;
    }
}
```

평소에 입력 기능을 위해 자주 사용하던 Scanner를 왜 다시 감싸줬을까? 뭐가 달라졌을까? 를 고민하다가 그럼 *요구사항대로 하지 않았을 때 어떤 상황을 방지하려고 이 요구사항을 추가했을까?* 를 생각해보았다.
결론은 **Scanner 개체 생성 남발을 방지하고 싱글톤 패턴을 위해서** 라고 판단했다.
<br/>
<br/>

## 2. camp.nextstep.edu.missionutils.Randoms
### pickNumberInRange()
```java
package camp.nextstep.edu.missionutils;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.Random;
import java.util.concurrent.ThreadLocalRandom;

public class Randoms {
    private static final Random defaultRandom = ThreadLocalRandom.current();

    private Randoms() {
    }

    public static int pickNumberInList(final List<Integer> numbers) {
        validateNumbers(numbers);
        return numbers.get(pickNumberInRange(0, numbers.size() - 1));
    }

    public static int pickNumberInRange(final int startInclusive, final int endInclusive) {
        validateRange(startInclusive, endInclusive);
        return startInclusive + defaultRandom.nextInt(endInclusive - startInclusive + 1);
    }

    public static List<Integer> pickUniqueNumbersInRange(
        final int startInclusive,
        final int endInclusive,
        final int count
    ) {
        validateRange(startInclusive, endInclusive);
        validateCount(startInclusive, endInclusive, count);
        final List<Integer> numbers = new ArrayList<>();
        for (int i = startInclusive; i <= endInclusive; i++) {
            numbers.add(i);
        }
        return shuffle(numbers).subList(0, count);
    }

    public static <T> List<T> shuffle(final List<T> list) {
        final List<T> result = new ArrayList<>(list);
        Collections.shuffle(result);
        return result;
    }

    private static void validateNumbers(final List<Integer> numbers) {
        if (numbers.isEmpty()) {
            throw new IllegalArgumentException("numbers cannot be empty.");
        }
    }

    private static void validateRange(final int startInclusive, final int endInclusive) {
        if (startInclusive > endInclusive) {
            throw new IllegalArgumentException("startInclusive cannot be greater than endInclusive.");
        }
        if (endInclusive == Integer.MAX_VALUE) {
            throw new IllegalArgumentException("endInclusive cannot be greater than Integer.MAX_VALUE.");
        }
        if (endInclusive - startInclusive >= Integer.MAX_VALUE) {
            throw new IllegalArgumentException("the input range is too large.");
        }
    }

    private static void validateCount(final int startInclusive, final int endInclusive, final int count) {
        if (count < 0) {
            throw new IllegalArgumentException("count cannot be less than zero.");
        }
        if (endInclusive - startInclusive + 1 < count) {
            throw new IllegalArgumentException("count cannot be greater than the input range.");
        }
    }
}
```

먼저 내가 사용해야 할 `pickNumberInRange()` 는 이름에서도 알 수 있듯이 범위 내의 랜덤한 수를 선택한다는 것을 알 수 있다. 코드 내부를 보면 `validateRange()`로 매개변수를 다시 전달하는 것을 알 수 있는데, 이 함수는 **지정한 범위가 유효한 지를 검사하는 함수**인 것을 알 수 있다.
그리고 다시 돌아가서 `defaultRandom.nextInt()`함수를 사용한다. 이 함수는 `java.util.concurrent.ThreadLocalRandom`를 타고 들어가보면 **시드 값 초기화를 할 필요 없이 정수인 난수를 생성**한다.

<br/>
<br/>
<br/>

[참고자료]<br/>
* [[Java] 일정 범위 이내의 정수인 난수를 자바에서는 어떻게 만들어야 하나요?](https://stanleykou.tistory.com/entry/Java-%EC%9D%BC%EC%A0%95-%EB%B2%94%EC%9C%84-%EC%9D%B4%EB%82%B4%EC%9D%98-%EC%A0%95%EC%88%98%EC%9D%B8-%EB%82%9C%EC%88%98%EB%A5%BC-%EC%9E%90%EB%B0%94%EC%97%90%EC%84%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%A7%8C%EB%93%A4%EC%96%B4%EC%95%BC-%ED%95%98%EB%82%98%EC%9A%94)
* [camp.nextstep.edu.missionutils.Console 의 메서드를 알아보자 - her0807/java-baseball-precourse GitHub Wiki](https://github-wiki-see.page/m/her0807/java-baseball-precourse/wiki/camp.nextstep.edu.missionutils.Console-%EC%9D%98-%EB%A9%94%EC%84%9C%EB%93%9C%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
* [[우아한테크코스 4기] 프리코스 1주차 미션](https://daram.tistory.com/243)