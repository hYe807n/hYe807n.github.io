---
title: "JSON"
layout: post
subtitle: SpringBoot를 위한 JSON 이해하기
category: studylog
tags: java spring
published: true
future: true
---

> **이 장에서 배울 내용**
>
> * JSON이란

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# > JSON(JavaScript Object Notation)
    : 오브젝트를 표현하기 위한 문자열
> 오브젝트 : 메모리상 존재하는 자료구조

JSON은 텍스트 문자열을 통해 오브젝트를 표현한다고 했다. A와 B 어플리케이션 사이에 데이터를 교환한다고 해보자. 이 때, 서로 사용하는 언어도 다르고 아키텍처도 다르다고 했을 때 이 <u>둘 모두 이해할 수 있는 형태로 변환</u>을 해야한다. 이를 위해 나온 것이 JSON으로 어떤 프로그래밍 언어로도 사용할 수 있다. 

그런데 모든 언어에서 사용할 수 있는데 왜 'JavaScript' Object Notation일까? 그 이유는 JavaScript에서 오브젝트를 생성하는 형식과 같기 떄문이다.

이렇게 메모리상의 오브젝트를 다른 형태로 변환하는 작업을 <u>'직렬화'</u>라고 하고, 이 반대를 <u>'역직렬화'</u>라고 한다. A 어플리케이션에 있는 오브젝트의 데이터를 B 어플리케이션이 필요하다고 할 때, A-object가 JSON 형태의 문자열로 직렬화된 후, HTTP 요청 바디 부분에 JSON을 넣어 요청을 전송한다. 그리고 요청을 받은 B 어플리케이션은 HTTP 바디 부분에서 JSON을 꺼내서 다시 역직렬화하여 사용한다. JSON 형태의 코드를 보자

## JSON 문법
```json
{
    "title":"myTitle",
    "author":"hYe8o7n",
    "hasTag":[  //문자열 배열
        "Hello",
        "World"
    ],
    "state":true
}
```
<u>JSON은 다음과 같이 key-value 형태로 직렬화</u> 할 수 있다. 인스턴스 변수의 이름~~(ex. title)~~은 key가 되고, 변수의 값~~(ex.myTitle)~~은 value가 된다. 배열을 사용할 때는 대괄호로 요소들을 감싸주면 된다. JSON에서 사용하는 자료형은 더 많으니 궁금하면 검색해보도록 하자.

[참고자료]
<br/>
*[JSON 기초 - 코딩의 시작, TCP School](http://www.tcpschool.com/json/json_intro_basic)  
*[JSON 기초 문법 정리 - 인파 - 티스토리](https://inpa.tistory.com/entry/JSON-%F0%9F%93%91-JSON-%EA%B8%B0%EC%B4%88-%EC%A0%95%EB%A6%AC)  
*[React.js, 스프링 부트, AWS로 배우는 웹 개발 101 | 김다정 - 교보...](https://product.kyobobook.co.kr/detail/S000001805062)