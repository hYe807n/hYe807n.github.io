---
title: 서버-정적, 동적 웹 서버 (서블릿 프로그램)
layout: post
summary: SpringBoot를 위한 정적&동적 웹 서버와 서블릿 프로그램
author: hYe8o7n
date: '2023-08-02 22:00:00'
category: java
keywords: "#DynamicWebServer #StaticWebServer #Servlet"
usemathjax: true
label: study
thumbnail: "/assets/img/posts/2023/08/20230803_dynamic.png"
published: true
future: true
---

> Server : 네크워크를 통해 클라이언트에게 정보와 서비스를 제공하는 프로그램

서버는 <u>네트워크 operation을 수행하는 것</u>으로 클라이언트가 소켓에 연결을 하면, 요청을 받아와 수행하고 응답을 작성해 전달한다. File Transfer Protocol을 사용하면 FTP server, HyperText Transfer Protocol을 사용하면 HTTP 서버가 되는 것이다. (*여기서 FTP는 파일 전송을 위한 프로토콜이고, HyperText Transfer Protocol은 사용자와 WebServer 사이의 문서 전송을 위한 통신 규약을 말한다.*)  

![Static wEB, Dynamic WEB](https://velog.velcdn.com/images%2Fdyunge_100%2Fpost%2F33a388a4-d96e-499b-b6e5-370ba3e216b6%2Fimage.png)

# Static Web Server
정적 웹 서버는  <u>리소스 파일을 반환하는 서버</u>이다. 클라이언트가 요청하면, 서버는 이미 저장된 리소스 파일~~~(ex. .png, .html등)~~~을 응답 바디에 넣어 전송한다. 따라서 같은 요청을 하는 사용자들에게 모두 같은 화면을 보여주고, 저장된 데이터가 변하지 않는 한 웹 페이지도 고정된다.

### 장점
* 다른 처리 없이 요청에 대한 파일만 전송하기 때문에 빠르다.
* 서버 설치, 설정하고 리소스를 경로에 지정하는 것 외에 개발자가 할 일이 없다.

### 단점
* 서비스가 한정적이다.
* 추가, 삭제, 수정 등의 작업을 코드로 직접 처리해야한다.

# Dynamic Web Server
동적 웹 서버는 <u>클라이언트의 요청을 처리한 후의 조건에 따라 웹 페이지를 재구성하거나 템플릿된 html 파일들 중 적절한 값을 대체</u>하여 응답 바디에 전송한다. 사용자는 시간, 조건, 상황, 요청 등에 따라 다른 구성의 웹 페이지를 보게 된다. 이러한 동적 웹 페이지의 구현을 도와주는 것이 바로 *서블릿 엔진/프로그램*이다.

### 장점
* 다양한 정보를 조합하여 데이터를 구성하기 때문에 서비스도 다양하게 제공될 수 있다.
* 사용자 측에서 수정, 삭제, 수정 등의 작업을 할 수 있기 때문에 관리가 용이하다.

### 단점
* 클라이언트에게 웹 페이지를 전송하기 전에 처리해야 하기 때문에 속도가 느리다.
* 웹 서버 외의 처리를 위한 어플리케이션 서버가 필요하기 때문에 비용이 든다.

**자바 서블릿 엔진**
서블릿 엔진은 동적 웹 페이지 구현을 위한 서버 프로그램이다. 개발을 하기 위해 서블릿 엔진이 이해할 수 있는 형식인 HttpsServlet을 상속 받는 클래스를 작성하여 어느 비즈니스 로직을 어떤 요청일 때 실행할 지 지정할 수 있다. 이렇게 하면 개발자는 서버를 처음부터 구현하지 않고도 비즈니스 로직을 구현하고 배포할 수 있다. *(스프링 부트 역시 내부적으로 서블릿 엔진을 사용한다고 한다.)*

[참고자료]
<br/>
*[서버가 뭔지 알기 쉽게 설명해줄게!](https://contents.premium.naver.com/3mit/wony/contents/220117230634163vp)
*[React.js, 스프링 부트, AWS로 배우는 웹 개발 101 | 김다정 - 교보...](https://product.kyobobook.co.kr/detail/S000001805062)
*[[WEB] 정적 웹 페이지와 동적 웹 페이지 - velog](https://velog.io/@dyunge_100/WEB-%EC%A0%95%EC%A0%81-%EC%9B%B9-%ED%8E%98%EC%9D%B4%EC%A7%80%EC%99%80-%EB%8F%99%EC%A0%81-%EC%9B%B9-%ED%8E%98%EC%9D%B4%EC%A7%80)
*[정적 웹페이지, 동적 웹페이지의 특징과 장단점 - 네이버블로그](https://blog.naver.com/insaweb/221650456057)
*[웹이라고 다 같은 웹이 아니에요, 정적 웹과 동적 웹 - 메일리](https://maily.so/grabnews/posts/ce76c9)
