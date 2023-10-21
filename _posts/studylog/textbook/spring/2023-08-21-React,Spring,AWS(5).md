---
title: "SpringBoot Annotation - @SpringBootApplication @Component @ComponentScan @Autowired @Bean"
layout: post
subtitle: "@SpringBootApplication @Component @ComponentScan @Autowired @Bean"
category: studylog
tags: java spring
published: true
future: true
---

> **이 장에서 배울 내용**
>
> * @SpringBootApplication, @ComponentScan
> * @Component
> * @Autowired
> * @Bean


<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}  



### 시작하기에 앞서  
[전 게시글에서](https://hye807n.github.io/studylog/ReactJs,Spring,AWS(4).html) SpringApplication을 처음 만들어봤다. 기본적으로 생성되는 src폴더 내부의 DemoApplication.java 파일을 보게되면 @SpringBootApplication이 달려있는데 무슨 의미일까?  
<br/>  
<br/>  
<br/>  

# 메인 메서드(DemoApplication)와 @SpringBootApplication @ComponentScan

```java
@SpringBootApplication
public class DemoApplication {	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```  
<br/>  

SpringBoot를 사용하는 가장 큰 이유 중 하나는 [DI(Dependency Injection) container]((https://hye807n.github.io/studylog/ReactJs,Spring,AWS(3).html)) 때문이라고 했다. <u>Spring은 basePackage와 그 하위 package들에서 java bean에 추가할 파일들을 찾아 DI container object에 추가</u>한다. 그리고 Application 실행 시 특정 object가 필요할 떄 그 object가 의존하는 다른 object를 찾아서 연결해준다.  
그런데 프로젝트 내에서 모든 Java Class와 field, method를 탐색하는 것은 비효율적이고 시간과 비용도 많이 소모될 것이다. 그래서 앞서 말했듯이 java bean에 추가할 파일들을 찾아 탐색하는 것은 basePackage와 그 하위 package들에서 해준다고 했는데, <u>baskPackage 위치를 설정해주는 것이 @ComponentScan</u> 이다. 그런데 **@SpringBootApplication annotation은 내부적으로 @ComponentScan을 포함**한다. 그렇기 때문에 @SpringBootApplication이 basePackage로 설정되어 메인 메서드에서 java bean에 추가할 오브젝트를 설정할 수 있는 것이다.  
<br/>  


<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbk517m%2FbtqEbuQ5gTp%2FsiwfXiHsfUd5Mt9d9nzNG1%2Fimg.png"></p>
그래서 실제로 @SpringBootApplication가 달려있는 basePackage를 기준으로 외부에 있는 package인 out 폴더 내에 있는 MyTestService는 DI container에 자동으로 추가되지 않아서 spring이 bean로 추가해서 관리해주지 않는다.
<br/>  
<br/>  
<br/>  

# @Component  

그럼 @ComponentScan이 붙은 package와 그 하위 package 내의 모든 object가 bean에 추가되는 것일까? 그것은 아니다. **탐색 범위 내에 있는 package에서 @Component annotation이 붙은 object들이 자동으로 java bean에 추가**되는 것이다.  
하지만 Spring 프로젝트 내에서 비교적 자주 접할 수 있는 @Controller, @Service, @Repository, @Aspect, @Configuration 등도 @Component를 포함하고 있기 때문에 java bean에 추가될 수 있다.  
<br/>  
<br/>  
<br/>  


# @Autowired  

@ComponentScan은 Bean들을 찾아 *BeanFactoryPostProcessor*를 적용하여 Bean으로 등록해주는 과정을 거치고 **@Autowired은 우리가 annotation을 통해 직접 bean에 추가했던~~~(@Service, #Configuration 등...)~~~ object들을 *BeanPostProcessor*의 구현체를 적용하여 의존성 주입을 적용하는 역할**을 하는 것이다.  
이렇게 앞서 말했던 @Component annotation을 통해 관리하고 싶은 class를 bean에 추가해주고 @Autowired annotation을 함께 이용해서 spring이 필요할 때 알아서 이 오브젝트를 생성해준다,  

> **BeanFactoryPostProcessor와 BeanPostProcessor**<br/>  
BeanFactoryPostProcessor는 bean에 대한 조작을 시작하기도 전에 처리하는 것이고, BeanPostProcessor는 bean이 생성되는 시점 - bean이이 초기화되는 시점(전,후)에서 처리된다.  
<br/>  
<br/>  
<br/>  


# @Bean

```java
@Configuration
public class ConfigClass {
    @Bean
    public Controller getController() {
        if (env == 'local') {
            return new LocalController(...);
        }
        return new Controller(...);
    }
}
```  

그런데 만약에 @Component 등을 통해서 Spring이 자동으로 관리해주는 것이 아닌 직접 관리하고 싶은 경우에는 어떻게 해야할까? 예를 들면 *Enterprise Application*의 경우에는 개발자가 object를 생성하고 어느 class에서 사용하는지 정확히 알아야 하는 경우가 많다. 이러한 경우나 Spring 기반이 아닌 라이브러리 class 를 bean에 추가하고 싶을 때 등... 의 경우 직접적으로 bean 생성 시 어떻게 처리할 지 코드로 작성해야한다.
이런 경우에 사용하는 annotation이 @Bean 이다.  <u>@Bean annotation을 사용해서 method를 정의해주면 spring에게 object를 어떻게 생성할 때 어떻게 처리해줄지 알려줄 수 있다.</u>



> **Enterprise Application**<br/>  
Enterprise Application이란 기업과 조직의 업무를 처리하는 서버에서 동작하는 application을 말한다.  
<br/>  
<br/>  
<br/>  
.<br/>
.<br/>
.<br/>
.<br/>
.<br/><br/>

다시 순서를 정리해보자.
- 1. Spring Application 실행함
- 2. @ComponentScan annotation을 baskPackage로 간주하여, 해당 package와 하위 package에서 @Component 요소 탐색하여 java bean에 추가함
- 3. @Component가 필요할 때 object를 생성하고 다른 object에 의존하면 찾아서 넣어주고 @Autowired를 사용하는 경우 Spring이 object를 연결함
- - 1. @Autowired에 연결된 Class가 @Component가 달린 class 일 경우 spring이 object를 생성함
- - 2. @Bean annotation으로 생성한 object인 경우 @Bean이 달린 method를 불러서 넘겨줌<br/>
<br/>
<br/>
<br/>

**글을 마치며...**<br/>
글 쓰는 법을 알기 위해서 문법을 공부해야하고 문법을 공부하기 위해서 단어를 알아야하는데 나는 아직 익숙하지 않은 단어도 많은 단계이다보니 아직 내용이 어렵게 느껴졌던 것 같다.  main method인 DemoApplication.java를 보다가 @SpringBootApplication annotation이 붙어있는 것을 발견하고 의문이 생겨서 알아보다보니, @ComponentScan을 이해해야하고 그러다보니 @Component를 이해해야하고.. 그러다보니 하나씩 하나씩 더 보게 됐는데 점점 파고들어가다보니 이 내용들을 한 번에 다 이해하기에는 버거워서 BeanFactoryPostProcessor, BeanPostProcessor 부터는 가볍게 이해하려고 했던 것 같다. 실습을 진행하면서부터는 annotation 사용에 조금 더 익숙해질테니 그 때 필요하다면 다시 봐도 괜찮을 것 같다.


<br/>
<br/>
<br/>
[참고자료]<br/>  
* [BeanPostProcessor vs BeanFactoryPostProcessormaketheworldwise·2022년 4월 10일](https://velog.io/@maketheworldwise/BeanPostProcessor-vs-BeanFactoryPostProcessor)
* [spring[5] ComponentScan과 Component](https://ililil9482.tistory.com/127)
* [[Spring] 컴포넌트 스캔 - @Component, @ComponentScan 사용하기](https://dct-wonjung.tistory.com/entry/Spring-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%8A%A4%EC%BA%94-Component-ComponentScan)
* [[Spring] @Component, 컴포넌트 스캔이란?](https://code-lab1.tistory.com/170)
* [[스프링 핵심기술] - @Component와 @ComponentScan](https://jjingho.tistory.com/9)
* [[Spring] 스프링 컴포넌트 스캔(Component Scan) 정리](https://ittrue.tistory.com/229)
* [JAVA Spring의 목적 - 끄적끄적 :: 끄적끄적 - 티스토리](https://ee-22-joo.tistory.com/7)