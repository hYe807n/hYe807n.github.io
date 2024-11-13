---
title: "자바 기초 이해하기-어노테이션(Annotation)"
layout: post
subtitle: "자바 어노테이션(annotation)"
category: studylog
tags: java
published: true
future: true
---

> **이 장에서 배울 내용**
>
> * Annotation 개념
> * Annotation 용도
> * Annotation 예시

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}


### 시작하기에 앞서  
Java-Spring 프로젝트를 시작하게 되면서 기본적으로 생성되는 코드(demoApplication.java)에 SpringBootApplication annotation이 붙어있었다.  지금껏 코드를 작성하면서 @Test, @Override, @Getter 등의 annotation을 자주 봤는데 annotation의 개념에 대한 이해가 필요하다고 생각해서 알아보게 되었다.  



# Annotation  
JDK 1.5 이상부터 사용 가능하고, Java version 5 부터 나온 기능이다. 기본적으로 주석이라는 의미를 가지고 있고~~(`//`, `#`와 같은 주석이 아니다.)~~ @Override @Test와 같이 @ 기호를 붙여서 사용한다.



## Annotation의 용도  
- 실행 시 특정 기능을 실행하도록 정보를 제공
- 빌드 시 코드를 자동으로 생성하도록 정보 제공
- 컴파일러에게 문법 에러를 체크하도록 정보를 제공
- 런타임 시 검사하는데 특정 코드를 사용  
  
등... 으로 다양하지만 가장 큰 비중을 차지하는 건 Class나 Method에 대한 <u>MetaData 제공</u>이라고 할 수 있다.  


> **MetaData**<br/>  
데이터를 위한 데이터를 의미하며 데이터에 대한 설명을 하기 위한 데이터이다. Application이 실행 시 처리해야할 데이터가 아니라 컴파일 과정 등에서 코드를 어떻게 처리할 것인지 방향을 알려주는 정보이다. *(예를 들면 개발자가 구현한 Class와 Method가 어떻게 구성되어있는지 등의 성격에 대한 설명)*  




## 내장 어노테이션 예시  

### @Deprecated  

```java
@Deprecated
public void oldOne() {
  ..
}
```  

@Deprecated는 field, method, type 등에 붙일 수 있고 @Deprecated annotation이 붙어있는 method나 field를 사용하면 build할 때 warining messaged가 뜬다. compiler에게 이 method는 곧 없어질 것이라는 것을 알려주고 경고하는 용도이다.  



.<br/>
.<br/>
.<br/>
.<br/>
.<br/><br/>  


  
비교적으로 자주 접할 수 있는 @Deprecated annotation을 예시로 보면 알 수 있듯이 <u>annotation은 비즈니스 로직에 직접적으로 영향을 주지 않고 Data에 대한 방향과 설명을 하기 위한 용도</u>로 자주 쓰인다.    
내장 annotation말고 costom annotation도 따로 만들어서 사용할 수 있지만 아직 사용할 일이 없었으므로 추후에 사용하게 되면 추가적으로 내용을 덧붙일 예정이다.   
<br/>
<br/>
<br/>


[참고자료]<br/>
* [[Java] Annotation 어노테이션 - 총정리](https://bangu4.tistory.com/199)
* [Java 어노테이션(Annotation) 이해](https://velog.io/@flasharrow/Java-Annotation-%EC%9D%B4%ED%95%B4)
* [Java - Annotation 이란? Annotation 사용법](https://galid1.tistory.com/534)
* [[Java] Annotation 의 동작의 이해](https://vixxcode.tistory.com/264)
* [Annotation 이해하기](https://bravenamme.github.io/2021/11/10/Annotation/)
* [메타데이터 - 박코딩](https://bakcoding.github.io/csharp/csharp-006-metadata/)
* [[Java] @Deprecated and @deprecated](https://blog.leocat.kr/notes/2017/04/08/java-deprecated-and-deprecated)