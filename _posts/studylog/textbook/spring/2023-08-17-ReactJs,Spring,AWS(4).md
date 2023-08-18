---
title: "Spring Boot 시작하기, SpringBootApplication"
layout: post
subtitle: SpringBoot 시작하기
category: studylog
tags: java spring
published: true
future: true
---

> **이 장에서 배울 내용**
>
> * SpringBoot 프로젝트 설정


<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# Spring initializr

먼저 Spring 프로젝트를 시작하기에 앞서 Spring Boot 에서 제공하는 툴인 [https://start.spring.io/](https://start.spring.io/)에서 원하는 라이브러리를 선택하고 하단의 Generate 버튼을 눌러서 생성한다.
![출처: https://honeywater97.tistory.com/256](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbJ3XRb%2Fbtrroykm8e0%2FMyVeyZah1N1MJDwKHOsRD0%2Fimg.png)

* Project: 생성할 프로젝트의 build tool을 선택한다. 선택지는 Maven, Gradle 두 가지가 있는데, **Spring을 처음 시작하는 Java 개발자라면 gradle을 추천**한다.  
gradle을 추천하는 이유와 Build Tool에 대한 이해가 필요하다면 [전 게시글](https://hye807n.github.io/studylog/gradle,maven.html)을 보자. ~~(나는 gradle을 선택했다.)~~  
* Language: 프로젝트에서 사용할 언어를 선택하면 된다. Java, Kotlin, Groovy 중 나는 Java Application을 만들 것이므로 Java를 선택했다.  
* Spring Boot: 프로젝트에서 사용할 Spring Boot 버전을 선택하면 된다.  
* Project Meta Data  
    * Group: 일반적으로 배포하는 회사 도메인을 입력한다.  ~~(하지만 내가 만들 프로젝트는 배포용이 아니므로 기본 설정인 com.example로 했다.)~~  
    * Artifact: 버전 정보는 생략된 빌드 후의 결과물인 jar 또는 war 형식의 파일 이름을 입력한다. ~~(Group과 마찬가지로 기본 설정인 demo로 했다.)~~  
    * Name: 프로젝트명을 입력한다. (일반적으로 Artifact와 동일하게 설정된다.)  
    * Description: Spring Boot 프로젝트 설명을 입력한다. ~~(나는 Demo project for Spring Boot로 설정했다.)~~  
    * Package name: 프로젝트 이름을 설정한다. 기본적으로 Group.Artifact 조합으로 설정되지만 변경 가능하다.  
    * Packaging: 배포 형태를 설정한다. 기본적인 선택지는 war, jar 두 가지가 있다. (war과 jar의 차이가 궁금하다면[전 게시글](https://hye807n.github.io/studylog/2023-08-18-war,jar.md)을 보자.) ~~(나는 jar로 선택했다.)~~  
    * Java: Java 버전을 선택한다. ~~(나는 11로 선택했다.)~~  
* Dependencies: 프로젝트에서 사용할 외부 라이브러리나 API 등을 추가할 수 있다. 목록에 추가하게 되면 Project에서 선택한 빌드 자동화 도구(gradle)이 관리해줄 것이다.  추가는 "ADD"나 "ADD DEPENDENCIES" 버튼을 눌러서 추가할 수 있다. ~~(내가 만들 Web project에서는 Spring Web, Spring Data JPA, H2 Database, Lombok을 추가했다.)~~    

**이제 `Genearte` 버튼을 누르고 압출 파일을 받아서 원하는 곳에 압축 해제하면 된다.**  

.<br/>
.<br/>
.<br/>
.<br/>
.<br/><br/>

압축 해제한 프로젝트는 eclipse에서 `project import`를 해주면 된다. (import 하는 과정은 사용하는 IDEA 마다 상이하므로 생략하므로 생략했다.) 그럼 src 폴더 내에 source code 파일이 생성되고, 단위 테스트를 실행할 수 있는 test 펄도와 gradle 파일이 생성된 것을 볼 수 있다.

![결과 이미지](https://github.com/hYe807n/hYe807n.github.io/blob/master/_posts/studylog/textbook/spring/img/2023-08-17/1.png?raw=true)

.<br/>
.<br/>
.<br/>
.<br/>
.<br/><br/>

**글을 마치며...**<br/>
Springinitializr를 통해 처음 프로젝트를 설정하고 생성하는 과정에서  무작정 책을 따라하는 것보다 왜 이걸 선택해야하는지를 알고 생성해야 나중에 혼자 프로젝트를 생성할 때도 수월하게 할 수 있을 것 같아서 모르는 것을 하나 하나 알아보느라 시간이 꽤 소요된 것 같다.  
지금에야 그냥 책을 따라하면 진도를 금방 나갈 수 있겠지만 혼자 프로젝트를 생성해야할 때가 오면 오늘 공부한 게 빛을 보지 않을까?  



[참고자료]<br/>  
* [React.js, 스프링 부트, AWS로 배우는 웹 개발 101, 김다정 - 교보...](https://product.kyobobook.co.kr/detail/S000001805062)  
* [Develop/Spring+JPA [Spring] Initializr로 스프링 부트 기반 프로젝트 생성](https://yeonyeon.tistory.com/67)  
* [Spring Initializr로 빠르게 시작하기](https://workshop.infograb.io/gitlab-ci/21_spring-boot_hello_world_rest_api/2_spring_initializr/)
* [[Spring Boot] spring initializr 사용하기](https://while1.tistory.com/104)