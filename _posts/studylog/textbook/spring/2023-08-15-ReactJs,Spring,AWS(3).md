---
title: "Spring"
layout: post
subtitle: Spring이 왜 필요한가
category: studylog
tags: java spring
published: true
future: true
---

> **이 장에서 배울 내용**
>
> * Spring
> * 의존성 주입
> * 제어의 역전
> * Spring Boot

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# > Spring
    : Java Application 개발을 하기 위한 오픈 소스의 경량 프레임워크

Python-DJango, Ruby-Ruby on Rails, Javascript-Node.js 처럼 Java 개발자들을 위한 웹 서비스 개발 프레임워크가 spring이다. Spring은 자바 객체와 라이브러리들의 Life Cycle 과 같은 것들을 관리하며 필요한 객체를 가져와 사용하기도 한다.

***프레임워크**<br/>
프레임워크는 개발자들이 개발을 편리하게 하기 위해서 관련 업무의 기술과 코드를 미리 만들어준 집합이다. ~~사용이 쉽고 간편한 경우 경량 프레임워크 라고 하기도 한다.~~

## DI(Dependency Injection)-의존성 주입
스프링의 가장 큰 특징 중 첫 번째는 바로 의존성 주입이다. 여기서 의존성이란, 
```java
public class TodoService {
    private final FileTodoPersistence persistence;

    public TodoService() {
        this.persistence = new FileTodoPersistence();
    }
}
```
Todo 목록들을 관리하는 Todo Application을 만든다고 해보자. 여기에는 Todo list들을 관리하는 TodoService 객체가 있다. <u>TodoService는 FileTodoPersistence를 활용하고 있고, TodoService는 FileTodoPersistence 없이는 제 기능을 할 수 없다. 이를 우리는 TodoService가 FileTodoPersistence에 의존</u>한다고 볼 수 있다.
나중에 개발이 더 진행되어서 100 개의 클래스가 쓰였는데 이제 File에 저장하지 않고 DataBase에 저장한다고 해보자. 그럼 우리는 수 많은 코드 줄 사이에서 100 개의 클래스들의 형식을 일일히 바꿔주어야 할 것이다. 심지어 저렇게 object 내에서 field 값을 초기화 하는 경우 단위 테스트 또한 어렵기 때문에 껍데기만 있는 클래스~~(Mock Class)~~를 사용해야한다. 이러한 문제점들을 해결하는 것이 <u>의존성 주입</u>이다. 대표적으로 두 가지 방법으로 구현되는데 **(1) 생성자를 이용해 주입**하는 방법과 **(2) Setter를 이용해 주입** 하는 방법이 있다.
```java
public class TodoService { // 생성자를 이용해 주입
    private final ATodoPersistence persistence;

    public TodoService(ATodoPersistence persistence) {
        this.persistence = persistence;
    }
}
```
```java
public class TodoService { // Setter를 이용해 주입
    private final ATodoPersistence persistence;

    public SetTodoService(ATodoPersistence persistence) {
        this.persistence = persistence;
    }
}
```
이처럼 생성자를 통해서 외부에서 넘겨받는 것을 의존성 주입이라고 한다. 이러한 것을 복잡하지 않고 전문적으로 해주는 것이 의존성 주입 컨테이너(Dependency Inject Container)라고 하고, 그 중 하나가 바로 스프링 프레임워크인 것이다.

## IoC(Inversion of Control)-제어의 역전
Java는 객체지향 언어인만큼 객체들끼리의 관계를 적절히 맺어주는 것이 중요하다.
```java
Class A {
    public void AMethod() {
        B b = new B();
        b.BMethod();
    }
}

Class B {
    public void BMethod() {
        ...
    }
}
```
다음과 같이 Class A 에서 B 객체를 생성하여 BMethod를 실행시킨다고 했었는데 새롭게 class C 객체의 CMethod를 사용하는 것으로 바꾼다고 생각해보자. B 객체를 생성하고, BMethod를 실행하는 두 줄은 C 객체를 생성하고 CMethod를 실행하는 줄로 바뀌어야할 것이다. 실무에서 다음과 같은 줄이 수십 줄이라고 한다면 꽤나 번거로운 작업이 될 것이다. 다음과 같이 바꿔서 작성해보면 좀 더 용이하게 사용할 수 있을 것이다.
```java
interface I {
    void example();
}
Class B implements I {
    public void example() {
        ...
    }
}
Class C implements I {
    public void example() {
        ...
    }
}
Class A {
    private I i;
    public A (I i){
        this.i = i;
    }

    public void AMethod() {
        i.example();
    }
}
```
다음과 같이 코드를 수정한다면 어떤 Class의 method라도 A 객체를 생성해주는 부분에서 유연하게 동작할 수 있을 것이다. 이처럼 스프링을 활용하면 로직 외부에서 A가 사용할 객체를 설정할 수 있다.
또한 실제로 스프링 부트가 클래스를 초기화할 때 어노테이션을 확인하고 이 클래스를 어떻게 관리하는지 알게되며 예시 코드처럼 new 키워드를 사용해 오브젝트를 일일이 생성하는 작업은 하지 않아도 된다.

.<br/>
.<br/>
.<br/>
.<br/>
.<br/><br/>

이처럼 개발자가 아닌 <u>스프링이 사용할 객체를 생성하여 의존 관계를 맺어주는 것을 IoC(제어의 역전)</u>이라고 하며, 그 과정에서 <u>객체의 생성자 혹은 Setter를 통해 주입해주는 것은 DI(의존성 주입)</u>이라고 한다.


# > Spring Boot
    : Spring을 더 쉽게 이용하기 위한 도구
위에서 대략 코드로 설명했다시피 Spring은 개발자들의 개발을 더 편리하게 할 수 있도록 해주는 프레임워크지만 Spring을 사용하기 위해서 이것저것 다양한 설정을 해줘야한다는 번거로움이 있다. 실제로 많은 개발자들이 스프링 부트가 나오기 이전까지 시작하기 어려워하는 경우도 있었다. <u>Spring Boot은 이러한 복잡한 설정을 직접 해줌으로써 개발자들이 비즈니스 로직에 더 집중하는 프로그래밍을 할 수 있도록 한다.</u>
스프링 부트를 이용하면 *스탠드 언론 프로덕션*급의 Spring 기반 Application을 쉬베 구동할 수 있다고 한다. **스탠드 언론이란, 다른 장치의 도움 없이 자체만으로도 Application을 실행할 수 있는 것을 말한다.** Spring Boot는 **(1) 임베디드 톰캣**이나 **(2) 제티** 같은 웹 서버를 Application 실행 시 함꼐 설정해준다. 개발자 입장에서는 적절한 어노테이션과 인터페이스 사용만으로 스프링 부트를 활용하여 비즈니스 로직에 집중하여 더 효율적인 프로그래밍을 할 수 있게 된다.

***임베디드 톰캣(Apache Tomcat)**<br/>
톰캣: Apache에서 만든 Web Application Server, Java sevlet과 JSP가 실행될 수 있는 환경을 제공하고 데이터를 동적으로 처리한다.
임베디드 톰캣(내장 톰캣): 더 간단하게 실행할 수 있지만 여러 호스트를 분기하는 기능을 구현하기에는 한계가 있어서 더 작은 단위의 프로젝트를 구현할 때 더 적합하다.

***제티(Jetty)**<br/>
경량 Web Application Server로 간단한 프로젝트를 관리하기에 빠르고 간단하지만 프로젝트가 커지면 트래픽~~(네트워크에서 일정 시간동안 흐르는 데이터의 양)~~ issue에 취약하다는 단점이 있다.

.<br/>
.<br/>
.<br/>
.<br/>
.<br/><br/>


**글을 마치며...**<br/>
Server 분야 공부는 기존에 해본 적이 없었고, 실습을 위해서 처음으로 가볍게 공부하다보니 배경지식도 없어서 공부하려고 읽는 글에도 모르는 용어 많아서 또 다시 돌아가 그 내용에 대한 공부를 하고.. 이러한 과정을 반복하다보니 비교적 짧다고 느낀 분량에도 불구하고 할애한 시간은 생각보다 길었던 것 같다.
아직 실제로 스프링을 사용해보지 못했다보니 간단한 예제 코드에 대한 이해를 해보려고 하기는 했는데 잘 이해한 것인지 궁금하다.
AOP, POJO도 Spring의 특징하면 자주 언급되는 내용인 것 같지만 현재 읽고 있는 책에서 따로 다루고 있는 내용은 아니라서 나중에 따로 자료를 찾아서 공부해보고 포스팅하면 좋을 것 같다.
아직 많이 부족하다는 생각이 들어서 마음은 조급하지만 요즘 기본적인 내용이 가장 중요하다고 느끼던 터라 급하게 생각하지 않고 천천히 꼼꼼하게 이해가 될 때까지 충분히 자료를 찾아보면서 공부하는 것이 좋을 것 같다.<br/><br/>

[참고자료]
<br/>
*[스프링과 스프링부트(Spring Boot)ㅣ정의, 특징, 사용 이유, 생성 방법](https://www.codestates.com/blog/content/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8)  
*[스프링(Spring), 스프링 부트(Spring Boot)란? 개념 정리](https://melonicedlatte.com/2021/07/11/174700.html)  
*[React.js, 스프링 부트, AWS로 배우는 웹 개발 101 | 김다정 - 교보...](https://product.kyobobook.co.kr/detail/S000001805062)