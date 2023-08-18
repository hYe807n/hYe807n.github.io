---
title: "Spring Boot - Ant, Gradle, Maven"
layout: post
subtitle: SpringBoot 시작하기
category: studylog
tags: java spring
published: true
future: true
---

> **이 장에서 배울 내용**
>
> * 빌드(Build)
> * 빌드 관리 도구(Build Tool)
> * 빌드 관리 도구의 필요성
> * Apache-Ant, Apache-Maven, Gradle 장단점
> * Maven vs Gradle 차이 비교

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

### 시작하기에 앞서
Spring 프로젝트를 시작할 때 주로 Sprig Boot 를 통해 프로젝트를 설정해주고는 하는데, 이 때 가장 먼저 spring initializr에서 프로젝트를 build할 tool을 지정해줘야한다. 선택지로는 gradle과 maven 두 가지가 있는데, 이 두 가지의 개념과 차이점이 뭘까?
~~사실 [내가 지금 공부하는 책](https://product.kyobobook.co.kr/detail/S000001805062)에서는 별도의 설명없이 gradle을 선택하라고 했지만, 나는 궁금한 것이 해결이 안 되면 다음 내용도 이해가 잘 안가는 편인지라 알아보게 되었다.~~

# 빌드 관리 도구(Build  Tool)

> **빌드(Build)**<br/>
우선 빌드는 소스코드 파일을 WAS(Web Application Server)가 인식할 수 있는 형식으로 변환하는 것이다.다시 말해, 우리의 소스코드(.java 형식)에서 사용한 라이브러리나 설정한 환경들을 정리해서 WAS가 인식할 수 있도록 패키징하는 것이다. 빌드 후에는 소스코드를 컴파일해서 .class 파일로 만들고 우리가 사용한 resource 들을 .class에 압축하는 과정을 거친다. *[(JVM의 구조와 흐름을 알고싶다면)](https://hye807n.github.io/studylog/young-java-study.html)*


먼저 maven과 gradle 모두 빌드 관리 도구(Build Tool) 중 하나이다. 그렇다면 빌드 관리 관리 도구는 무슨 일을 할까? 
다양한 외부 라이브러리들을 활용한 개발이 가능해지면서, 다양한 라이브러리들을 사용하면서 개발자들이 하나 하나 관리하게 되면 버전 관리, 다운로드한 라이브러리들끼리의 의존성과 빌드 순서를 고려하면 굉장히 복잡하다. 그렇기 때문에 이러한 <u>복잡한 필드 프로세스를 직접 개발자가 관리할 필요 없이 자동으로 관리해주는 것이 빌드 관리 도구</u>이다. 
빌드 관리 도구는 다음과 같은 작업을 수행한다.
> 1. 종속성 다운로드 - **전처리**
> 2. 소스코드를 바이너리 코드로 변환 - **컴파일**
> 3. 바이너리 코드를 **패키징**
> 4. **테스트** 실행
> 5. 시스템에 **배포**



## Apache-Ant
<p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fk0RHU%2FbtrB59Ov1hj%2FIHp1aepHi33fKXg2gnjiHk%2Fimg.png"></p> 
먼저, Apache의 Ant가 가장 먼저 출시되었다. build.xml로 xml 형식을 기반으로 스크립트를 작성하고, 규칙이나 **구조가 정해져있지 않아서 유연성**이 있었다.
하지만 개발자가 프로젝트 내에서 **빌드하는 모든 명령을 직접 작성해야하고 종속성 관리가 되지 않았다**. 또한 규칙이 없다는 유연성이 **build file을 이해하는데 시간이 걸리는 단점**이 되기도 했다. 이러한 단점은 프로젝트의 규모가 커질수록 치명적이다.
그렇기 때문에 지금은 Maven이 Ant의 대안으로 나오게 되었다.



## Apache-Maven
<p align="center"><img src="https://velog.velcdn.com/images/limsubin/post/35c62aec-e65c-423a-b502-6dcc5d3f8707/image.png"></p>  
Java8용 프로젝트 관리 도구로 앞서 말한 Ant의 대안으로 만들어졌다. Apache license로 배포되는 오픈 소스 라이브러리로 **Ant와는 다르게 정형화된 빌드 시스템이 제공돼서 규칙성이 있다는 특징**이 있다. 
**정해진 라이프 사이클(Life Cycle)에 의해 작업을 수행**하며 **전반적인 프로젝트 관리 기능을 포함**하고 있다.~~(때문에 BuildTool+Project Management 기능을 한다고 표현한다.)~~ 
때문에 다양한 플러그인을 통해 많은 작업이 **자동화** 되고, **의존성 라이브러리 관리**가 편하다는 장점이 있다.  
하지만 <u>Maven의 특징인 규칙성은 Ant에 비해 유연성이 떨어진다는 단점</u>이 되고, Maven에서 지원하지 않는 build 과정을 추가해야하는 경우 복잡하다.



## Gradle
<p align="center"><img src="https://velog.velcdn.com/images/orijoon98/post/cbb8f7e2-627b-4202-b890-bddd2a1c0ea0/gradle.png"></p>  
앞서 말한 Ant, Maven의 개념을 적용하여 단점을 보완하고 장점을 취합하여 나온 대안이다.  
Ant&Maven은 .xml file 형식으로 작성되는 반면에 **Java와 문법이 비슷한 Groovy 언어**를 기반으로 작성되는 소스 파일로 작성되어 Java 개발자들이 더 편리하게 사용할 수 있다. 또한 XML과는 다르게 변수 선언, if, else, for 등의 **로직이 구현 가능**하므로 가독성이 좋다. 또한 **기존의 Maven을 완전 지원**하고, **JAVA 뿐만 아니라 C/C++, Python도 사용**할 수 있다.  

> **Groovy**<br/>
Groovy는 gradle 파일을 작성할 때 사용하는 스크립트 언어이다.~~(gradle 파일을 작성하기 위한 언어로 gradle dsl(DSL: 도메인 고유언어-특정 용도에 한정된 언어)라고 불리기도 한다.)~~ JVM에서 실행되는 스크립트 언어이지만 Java와 달리 별도의 컴파일 과정이 필요 없이 sourcecode를 그대로 실행할 수 있다. 또한 Java와 호환되고, Java와 문법이 비슷하여 Java 개발자들이 사용하기에 편리하다.



## Gradle vs Maven
우선 [gradle 측에서 Maven과 비교한 공식 문서](https://gradle.org/maven-vs-gradle/)까지 있는 걸 보면 gradle이 Maven과 비교했을 때 자신이 있다는 것을 알 수 있다. 그렇다면 어떤 장점이 있을까?  
우선 한눈에 봐도 **스크립트 길이와 가독성 면에서 gradle이 더 우세**하다는 것을 알 수 있다. 또한 gradle은 로직이 구현하기 때문에 코딩에 의한 **간결한 정의**가 가능하다.  
<details>
  <summary>pom.xml</summary>
  <div markdown="1">
  
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.2</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example2</groupId>
    <artifactId>demo-maven</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo-maven</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>11</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
 
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
 
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
    </project>

</div>
</details>

<details>
    <summary>build.gradle</summary>
    <div markdown="1">
    plugins {
    id 'org.springframework.boot' version '2.5.2'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
    }
 
    group = 'com.example'
    version = '0.0.1-SNAPSHOT'
    sourceCompatibility = '11'
 
    repositories {
        mavenCentral()
    }
    
    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter'
        testImplementation 'org.springframework.boot:spring-boot-starter-test'
    }
    
    test {
        useJUnitPlatform()
    }
</div>
</details>  


![출처 : https://velog.io/@leesomyoung/Maven%EA%B3%BC-Gradle%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EB%B0%8F-%EB%B9%84%EA%B5%90](https://velog.velcdn.com/images/leesomyoung/post/f1f24aa9-dec0-4900-adb6-8334efa15b1c/image.png)<br/>  

build과 test 실행 결과 gradle이 훨씬 빠르고, **gradle은 cache를 사용하기 때문에 test 반복 시 차이가 더 커지는 걸 알 수 있다.** Maven의 경우 프로젝트가 커질수록 설정 내용이 길어지고 스크립트 내용이 배로 길어진다.  
**Maven은 상속 구조를 이용**해서 멀티 모듈을 구현하는데, 이는 특정 설정을 모듈끼리 공유하기 위해서 부모 프로젝트를 상속하게 해야하여 복잡하다. 반면 **gradle은 설정 주입 방식**을 사용하여 멀티 프로젝트에 적합하다.  
때문에 <u>gradle이 maven보다 최대 100배는 빠른</u> 속도를 보인다고 하다.

.<br/>
.<br/>
.<br/>
.<br/>
.<br/><br/>

이러한 장단점으로 인해 요즘은 Maven 보다 gradle을 많이 사용하는 추세이며, 처음 사용하는 사용자에게도 gradle이 더 적합하다는 의견이 많고, 현 시점에서 gradle을 사용하지 않을 이유는 기존 Maven을 사용하는 개발자들의 익숙함 뿐이라고 한다. Ant의 유연성과 Maven의 편리한 의존성 관리 기능을 합쳐 놓은 gradle은 버전이 올라가며 성능 또한 많이 개선되어 대세가 되었다.<br/>
<br/>
<br/>
<br/>

**글을 마치며...**<br/>
지금 [내가 보고 있는 책](https://product.kyobobook.co.kr/detail/S000001805062)에서는 별도의 설명 없이 spring 프로젝트를 설정할 때 빌드 관리 도구로 gradle을 선택하라고 했는데, 막상 사이트를 들어가보니 선택지가 maven과 gradle 두 가지가 있었다.  
기존에 java 프로젝트를 진행할 때에도 gradle file이 자주 보였던 터라 궁금하던 찰나에 이왕 공부할 거 두 가지의 차이점에 대해 알고 있으면 이해하는데에 도움이 될 것 같아서 공부하게 되었다.  
Maven, Gradle을 알아보면서 빌드 관리 도구에 대한 개념도 알게 되었는데, 지금이야 gradle이 워낙 많이 쓰여서 나도 당연히 쓰는 구나 하고 써왔었는데 그 전에 Maven과 Ant가 존재했고 문법을 비교해보니 gradle 덕분에 관리를 훨씬 편하게 할 수 있었구나 하고 알게 되었다. 아직 완벽히 이해했다고 말하기는 힘들지만 그래도 지금껏 Java를 사용하면서 계속 딸려있던 gradle의 존재 이유와 필요성에 대해서 알고나니 더 도움이 될 것 같다.  

[참고자료]<br/>
* [Maven 보다 Gradle을 선호하는 이유를 알아보자!](https://velog.io/@limsubin/Maven-%EB%B3%B4%EB%8B%A4-Gradle%EC%9D%84-%EC%84%A0%ED%98%B8%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
* [Maven과 Gradle의 차이](https://hyojun123.github.io/2019/04/18/gradleAndMaven/)
* [Maven과 Gradle의 개념 및 비교](https://velog.io/@leesomyoung/Maven%EA%B3%BC-Gradle%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EB%B0%8F-%EB%B9%84%EA%B5%90)
* [메이븐(Maven)과 그래들(Gradle)의 개념 및 비교](https://dev-coco.tistory.com/65)
* [JAVA의 빌드툴에 대해 알아보자](https://doosicee.tistory.com/entry/JAVA%EC%9D%98-%EB%B9%8C%EB%93%9C%ED%88%B4%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
* [Maven 이란?](https://velog.io/@changyeonyoo/Maven-%EC%9D%B4%EB%9E%80)
* [Gradle vs Maven Comparison](https://gradle.org/maven-vs-gradle/)