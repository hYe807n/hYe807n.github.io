---
title: "[우아한 테크코스-프리코스 3주차] 단위 테스트 JUnit5 AssertJ"
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
![과제 진행 요구사항](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/5214c181-4c3b-46d1-90fb-71d3383764e9)

우아한 테크코스의 프리코스 3 주차 미션의 요구사항에 AsserJ와 JUnit 5 를 활용하여 `도메인 로직에 단위 테스트 구현`하는 것이 추가되었다. 
이를 위해 이번 미션에서 활용된 주요한 기능들을 위주로 정리해보았다.
<br/>
<br/>

# JUnit5
***
## @DisplayName
```java
@DisplayName("<구입 금액의 단위가 1000 원이 아니라면(1111 원) 예외를 발생한다.>")
```
Test class 혹은 Test Method의 이름을 지정하여 각 테스트에서 어떤 작업을 하는지 테스트 결과에서 명시적으로 볼 수 있다. 어떤 작업을 수행하여 어떤 결과가 나와야하는 지 등 **상세하게 적는 것을 지향**한다.
<br/>

## @ParameterizedTest
여러 argument 를 이용해 인자가 다르지만 중복되는 코드를 사용한 테스트를 하나의 테스트 코드를 통해 여러 번 돌릴 수 있는 기능을 한다. **`@Test` 대신 `@Parameterized` 를 붙여서 사용**한다. 파라미터에 들어갈 source를 넣어줄 source annotation 중 하나를 사용해야하고, `@ValueSource`, `@EnumSource`, `@MethodSource` 등이 있다.
<br/>

### @MethodSource
```java
static Stream<Arguments> provideScore() {
        return Stream.of(
            Arguments.of(6, Price.FIRST),
            Arguments.of(5, Price.SECOND),
            Arguments.of(4, Price.FORTH),
            Arguments.of(3, Price.FIFTH)
        );
}

@DisplayName("당첨 번호 개수가 {0} 개 라면, Price.{1} 객체를 반환한다.")
@ParameterizedTest
@MethodSource("provideScore")
void checkPriceByScore(int score, Price price) {
        assertThat(Price.findByScore(score)).isEqualTo(price);
}
```
Test class 내의 Method 나 외부 class Method 가 반환하는 값을 source로 하는 것이다. Test class 내에 있고 `@TestInstance` 를 붙인 것이 아니라면 static Method 여야 한다.
Test Method 와 식별자가 동일하면 source를 명시적으로 적지 않아도 된다. 인자로써 사용할 Method 명을 적어주면 된다.
`provideScore()` 에서 넘겨준 4 개의 `Argement` 를 차례대로 받아서 `int score`, `Price price` 인자에 값을 받는다. `@DisplayName` 을 통해서 인자 값을 테스트 결과에 포맷할 수도 있다.
<br/>
<br/>

# AssertJ
***
## assertThatThrownBy()
```java
    @DisplayName("Bonus 번호에 숫자 외의 문자가 입력되면 예외를 발생한다.")
    @Test
    void createBonusByNotNumber() {
        assertThatThrownBy(() -> new Bonus("50a", answer))
            .isInstanceOf(IllegalArgumentException.class);
    }
```
Java 8 람다식을 사용하여 예외를 더 쉽게 검출하기 위한 기능이다. 반드시 예외를 발생하는 코드를 인자로 넘겨주어야한다. 만약 예외가 발생하지 않는다면 Error 가 발생한다.
<br/>

### isInstanceOf()
`assertThatThrownBy()` 에서 발생한 예외가 어떤 종류인지 인자로 넘겨 `.class` 형태로 입력한다.
<br/>

## assertThatIllegalArgumentException()
```java
    @DisplayName("로또 번호의 범위가 1부터 45가 아니라면 예외가 발생힌다.")
    @Test
    void createLottoByOverRange() {
        assertThatIllegalArgumentException().isThrownBy(() ->
            new Lotto(List.of(1, 2, 3, 4, 5, 50)));
    }
```
AssertJ 에서 자주 발생하는 예외들에 대해서 정의한 함수이다. 대표적으로 `assertThatNullPointerException`, `assertThatIllegalArgumentException`, `assertThatIllegalStateException`, `assertThatIOException` 가 있다.
<br/>

### isThrownBy()
`assertThatIllegalArgumentException()` 과 같은 함수에서 예외가 발생할 코드를 `isThrownBy()`의 인자로 작성하면 된다.
<br/>

## assertThat()
```java
@DisplayName("Lotto 숫자를 오름차순으로 정렬하여 반환한다.")
    @Test
    void sortNumbers() {
        assertThat(new Lotto(List.of(6, 5, 4, 3, 2, 1)).getNumbers())
            .containsExactly(1, 2, 3, 4, 5, 6);
    }
```
두 가지 값을 비교하기 위한 기능으로, 첫 번째 인자로 비교 대상 값을, 두 번째 파라미터로 비교 로직이 담긴 Mather가 사용된다. 혹은 관련 Method를 체인닝하여 사용할 수도 있다. 대표적으로 `isEqualTo()`, `contains()`,` doesNotContain()`, `startsWith()`, `containsExactly()`, `isNotEmpty()`, `isPositive()`, `isEqualTo()` 등이 있다.
<br/>

### containsExactly()
- **`containsExactly()` : 중복 여부, 순서, 원소 값과 개수 모두 일치해야 성공**
> 고려해볼만한 유사한 메소드
> - `contain()` : 중복 여부, 순서, 개수에 관계 없이 값만 일치하면 성공
> - `containsOnly()` : 중복 여부, 순서 관계 없이 값만 일치하면 성공, 원소 값과 개수가 정확히 일치해야 함
<br/>

### isEqualTo()
```java
    @DisplayName("구입 금액이 5000 원이라면 구매한 로또 개수가 5 개이다.")
    @Test
    void lottoCountByMoney() {
        assertThat(new Money("5000").countLottos()).isEqualTo(5);
    }
```
대상의 내용을 비교하기 위한 기능이다. **유사한 메서드로는 `isSameTo()` 가 있고, 이는 주소값을 비교하는 메서드이다.** 주소값이 아닌 값을 확인하기 위한 용도로 사용했다.
<br/>

## Assertions
```java
    @DisplayName("로또 번호에 보너스 번호가 포함되어 있으면 true 를 반환한다.")
    @Test
    void lottoHasBonus() {
        Assertions.assertTrue(new Lotto(List.of(1, 2, 3, 4, 5, 6)).hasBonus(5));
    }
```
`boolean` 식을 포함하고 있는 문장으로서, 주로 논리적으로 불가능한 상황을 확인하는 데 사용된다.
<br/>

### assertTrue()
파라미터의 코드가 `true` 를 반환하면 테스트를 성공시킨다.
<br/>

### assertFalse()
파라미터의 코드가 `false` 를 반환하면 테스트를 성공시킨다.
<br/>

### assertEquals()
두 파라미터의 객체를 확인하여 일치하면 테스트를 성공시킨다.
<br/>
<br/>

# System.out.printLn 출력 테스트
***
```java 

    private static final ByteArrayOutputStream output = new ByteArrayOutputStream();

    @BeforeEach
    public void setUpStreams() {
        System.setOut(new PrintStream(output));
    }

    @AfterEach
    public void restoreStreams() {
        System.setOut(System.out);
        output.reset();
    }

    @DisplayName("로또를 5 개 구매했을 때, 양식에 맞춰서 구매 개수를 출력한다.")
    @Test
    void printLottoCountResultInForm() {
        OutputView.printLottoCount(5);
        assertEquals("5개를 구매했습니다.", output.toString().trim());
    }
```
<br/>

## @BeforeEach @AfterEach
각 테스트 메소드가 실행되기 전과 후에 매번 실행되어야 하는 메소드에 붙여 사용한다. 초기화 작업이나, 테스트 환경을 설정하고 메소드가 실행된 후에 사용한 데이터들을 정리하거나 상태를 복원시켜놓는 작업을 처리한다.
<br/>

## ByteArrayOutputStream
**OutputStream의 일종으로, 데이터를 바이트 배열에 쓰는 데 사용**됩니다. 위의 코드에서 `System.out` 출력을 저장하기 위해 사용한다.
<br/>

## PrintStream()
`System.out.println()`과 같은 메소드를 호출하여 출력할 때, 이를 `output` 스트림에 데이터를 담아서 사용할 수 있도록 한다. `ByteArrayOutputStream` 인스턴스를 받아서 `PrintStream` 으로 감싸서 바이트 출력 stream을 문자 출력 stream으로 변환한다.
<br/>

## System.setOut()
`System.out`을 통해 출력된 문자열을 사용자가 정의한 `PrintStream`으로 변경되어 콘솔 대신 `ByteArrayOutputStream`으로 데이터를 보낸다.
<br/>

## output.reset()
다음 테스트가 실행될 때 이전 출력 데이터가 남아있지 않도록 `ByteArrayOutputStream `인스턴스를 초기화한다.
<br/>
<br/>

# 내부 API 
***
## assertRandomUniqueNumbersInRangeTest()
```java
@DisplayName("랜덤 번호가 1~45 범위에 중복 없이 6 개의 숫자 리스트를 반환한다.")
    @Test
    void returnUniqueLottoGenerator() {
        assertRandomUniqueNumbersInRangeTest(
            () -> {
                assertEquals(Number.LottoGenerator(), List.of(8, 21, 23, 41, 42, 43));
            },
        List.of(8, 21, 23, 41, 42, 43)
        );
    }
```
```java
    public static void assertRandomUniqueNumbersInRangeTest(
        final Executable executable,
        final List<Integer> value,
        final List<Integer>... values
    ) {
        assertRandomTest(
            () -> Randoms.pickUniqueNumbersInRange(anyInt(), anyInt(), anyInt()),
            executable,
            value,
            values
        );
    }
```
```java
    private static <T> void assertRandomTest(
        final Verification verification,
        final Executable executable,
        final T value,
        final T... values
    ) {
        assertTimeoutPreemptively(RANDOM_TEST_TIMEOUT, () -> {
            try (final MockedStatic<Randoms> mock = mockStatic(Randoms.class)) {
                mock.when(verification).thenReturn(value, Arrays.stream(values).toArray());
                executable.execute();
            }
        });
    }
```
우테코 내부 API인 `Randoms.pickUniqueNumbersInRange()` 에서 반환할 요소들을 지정하여 확인하기 위한 용도로 사용한다. 내부 API 이므로 `ctrl+B` 를 타고 내부로 들어가서 코드를 확인해볼 수 있다. *`pickUniqueNumbersInRange()`는 [이전 게시글](https://hye807n.github.io/review/woowacourse-precourse-6-java-baseball-3.html) 에서 리뷰 포스팅하였다.*
<br/>
<br/>

## 궁금증
***
```java
assertThat(new Money("5000").countLottos()).isEqualTo(5);
Assertions.assertTrue(new Money("5000").countLottos() == 5);
```
단위테스트 코드를 작성하다가 위의 코드처럼 `Assertions.assertTrue()`/`Assertions.assertFalse()`는 `assertThat().isEqualTo()` 로도 사용할 수 있지 않을까? 라는 생각이 들었다. 하지만 `assertTrue()` 함수를 사용하는 이유는 다음 코드가 `True`/`False` 값을 확인한다는 것을 명시적으로 드러냄으로서 가독성을 높일 수 있기 때문이였다.
<br/>
<br/>

# 클래스 분리 - 단위 테스트
***
클래스를 목적에 맞게 분리하고, 함수를 작은 단위로 쪼개는 것은 결국 테스트 코드 역시 작은 단위씩 테스트할 수 있도록 한다. 즉, 작은 단위부터 테스트를 하기 위해서는 함수 역시도 작은 단위로 쪼개야 테스트하기가 더 쉬워진다. 만약 여러 기능과 정보가 하나의 class 혹은 method에 한 번에 들어있다면 테스트 구현이 복잡해지고 때로는 불가능할 수도 있다. 
또한 큰 단위의 테스트를 구현하기 전에 먼저 작은 단위 테스트부터 진행하여 먼저 잘못 구현한 것이 없는지 확인한 후에, 더 큰 테스트를 구현하여 조금씩 확인할 범위를 확장해야 수정할 부분을 더 빠르게 확인할 수 있다.
<br/>
<br/>
<br/>

[참고자료]<br/>
* [AssertJ 자주 사용하는 것들](https://umanking.github.io/2021/06/26/assertj-iteration/)
* [완벽정리! Junit5로 예외 테스트하는 방법](https://covenant.tistory.com/256)
* [[Junit] Junit5 Exception Throw 테스트](https://small-stap.tistory.com/77)
* [AssertEquals(String, String) ComparisonFailure when contents are identical](https://stackoverflow.com/questions/36324452/assertequalsstring-string-comparisonfailure-when-contents-are-identical)
* [@ParameterizedTest로 한 번에 테스트하자](https://velog.io/@ohzzi/junit5-parameterizedtest)
* [[JUnit] @ParameterizedTest - @MethodSource 사용하기](https://ebabby.tistory.com/28)
* [[Java] JUnit 5 사용법 (5) - @EnumSource, @MethodSource, @CvsSource](https://effortguy.tistory.com/117)
* [TDD로 개발하기 4탄](https://velog.io/@kyle/TDD%EB%A1%9C-%EA%B0%9C%EB%B0%9C%ED%95%98%EA%B8%B0-4%ED%83%84)
* [우테코 2주차 JUnit5 Test 코드 해석](https://devbasket.tistory.com/m/66)
* [Unit Testing of System.out.println() with JUnit](https://www.baeldung.com/java-testing-system-out-println)
* [JUnit test for System.out.println()](https://stackoverflow.com/questions/1119385/junit-test-for-system-out-println)
* [프로그래밍/JAVA[Java] 단위테스트: System.out.println() 과](https://choichumji.tistory.com/118)