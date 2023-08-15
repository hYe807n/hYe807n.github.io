---
title: "자바 기초 이해하기-JVM, JRE, JDK"
layout: post
subtitle: "자바 기초 이해하기"
category: studylog
tags: java
published: true
future: true
# related_posts:
#   - _posts/textbook/procpp/2021-08-06-professional-cpp-toc.md
---

> **이 장에서 배울 내용**
>
> * 컴파일, 런타임 환경에서의 JVM
> * JVM 구조와 흐름(Execution Engine)
> * JRE
> * JDK

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# > JVM(Java Virtual Machine)
    :자바 프로그램의 실행환경을 만들어주는 가상 머신

JVM을 사용하면 플랫폼에 구애받지 않고 동작할 수 있다. 이게 다른 언어와 다른 점이 무엇이냐하면, 
(*실행구조 순서대로 글을 보고싶다면 숫자(1)(2)...가 붙여져있는 부분을 위주로 보자.*)<br/><br/>


## 컴파일 환경
먼저 예를 들어 윈도우 환경에서 C 언어로 작성된 .c 파일 같은 경우에는 c 언어 소스 코드를 **(1) 컴파일러를 통해 기계어로 변환하여** **(2) 윈도우에서 실행 가능한 .exe 실행파일**로 만든다. 이렇게 만들어진 실행 파일은 리눅스와 같이 다른 플랫폼 환경에서 실행시킬 수 없다.

하지만 JAVA의 경우에는 **(1)java 컴파일러를 통해 JVM이 이해할 수 있는 바이트 코드(.class)로 변환**한다. 그리고 이렇게 생성된 바이트 코드를 각각의 플랫폼에서 동작할 수 있는 자바의 가상 머신 (JVM)이 실행 가능한 파일로 만들어준다. 즉, <u>컴파일을 플랫폼에 구애받지 않고 수행할 수 있다는 말이다.</u>

하지만 JVM은 각각의 플랫폼에 맞는 버전을 설치해줘야한다.<u>(이를 "JVM이 플랫폼에 종속적이다." 라고 표현을 하기도 한다.)</u>

**바이트 코드란?**
사람이 작성한 코드보다 더 기계친화적인 언어로, 특정 하드웨어에서가 아닌, 가상머신이 실행할 수 있도록하는 이진 표현법이다. 대표적으로 자바와 C#이 이런 바이트 프로그램을 사용한다. 이는 프로그램 실행 전에는 인터프리터보다 느릴 수 있지만 성능이 훨씬 더 좋다. ~~위에서 설명했듯이, 이렇게 생성된 바이트 코드는 플랫폼에 구애받지 않고 가상머신이 실행 가능한 파일로 만들어줄 수 있다.~~<br/><br/>


## 런타임 환경
![JVM 구조](https://velog.velcdn.com/images%2Fbongf%2Fpost%2F204c2b42-b25b-4fc6-aec5-4a4269e6f260%2Fimage.png)
이렇게 만들어진 바이트코드(.class)를 **(2)클래스 로더가 JVM Runtime Data Area(메모리)에 할당**한다. <u>(JVM Runtime Data Area는 운영체제에서 프로그램을 실행시키기 위해서 할당하는 메모리 영역이다.)</u>   다섯 영역의 역할을 하나씩 살펴보자.

여기서 가장 앞 쪽의 Method, Heap Area는 모든 thread에 공유가 되고 나머지 Stack Area, Native Method Stack, PC Register는 thread가 별도로 생성이 된다.

### **1. Method Area**
프로그램 내에서 코드로 구현된 클래스는 처음부터 저장되지 않는다. 클래스가 할당이 되어 객체가 생성이 되면 JVM은 클래스 함수, 필드 등을 이 공간에 저장한다. 저장되는 정보들은 필드, 메소드, 클래스 정보, <u>static 변수</u> 등등으로 아주 많으니 자세한 내역이 알고 싶다면 검색 해보도록 하자.

### **2. Heap Area**
프로그램은 실행될 때 heap 영역을 많이 할당해놓는데 주로 정적 개체가 생성될 때 이 공간으로 간다. reference 변수의 경우 이 heap에 instance 공간이 할당되는 것이 아니라 포인터가 할당된다.~~(heap 공간에 대한 자세한 이야기는 다른 포스팅으로 따로 정리할 것이다.)~~  

### **3. Stack Area**
쓰레드 제어를 위해 할당되는 공간으로, method가 실행될 때마다 쓰레드가 하나씩 배정된다. method가 호출 되면 method 정보(method 매개변수, local 변수 등)가 기록되며, method 호출이 종료되면 메모리 공간이 사라진다.

### **4. PC Register**
CPU 같은 경우에는 연산 과정에서 필요한 정보들을 Register라는 공간에 담아둔다. 예를 들면 1 + 2 라는 연산을 수행하기 위해 1과 2를 공간에 넣어두고 더하는 연산을 실행하고 이 결과 또한 다른 Register 공간에 할당하는 것처럼 말이다.
JVM에서는 코드를 수행하는 과정에서 프로그램의 실행 상태를 저장하기 위해 PC Register라는 공간을 사용하는데 실행 과정에서 별도의 프레임에 지역 변수와 중간 연산 결과를 저장한다. ~~(실제 하드웨어의 Register에 저장되는지에 대한 여부는 코드의 상황에 따라 다르며  JVM과 운영체제 간의 추상화를 통해 처리한다.)~~ 

### **5. Native Method Stack**
앞서 Java로 구현된 코드는 플랫폼에 구애받지 않고 실행될 수 있다고 했다. 하지만 운영체제의 모든 기능을 Java로 구현할 수는 없어서 다른 언어로 작성된 Method를 사용해야할 수도 있다. 
이를 대처하기 위한 방법이 JNI(Java Native Interface)이다. JNI를 통해 보통 C나 C++로 구현되어있는 운영체제의 고유 함수들을 Java Method와 연결하여 Java method를 호출해도 C나 C++ 함수가 실행될 수 있도록 한다. 이러한 정보들이 담겨있는 곳이 NMS(Native Method Stack)이다.

이렇게 JVM Runtime Data Area에 로딩된 **(3).class 데이터들은 Execution Engine을 통해 변환**이 된다.<br/><br/>


## Execution Engine
Exucution Engine은 Interpreter, JIT(Just In Time) 두 가지 방법을 함께 사용하여 변환한다. 먼저 Interpreter 방법은 가장 초기에 사용됐던 방법으로 한 줄씩 코드를 읽고 실행시키는 방법인데 속도가 느리다는 단점이 있다. 이런 단점을 보완하고자 추후에 나온 것이 JIT이다. JIT는 프로그램에서 자주 실행되는 코드를 미리 준비해서 저장해둔 다음 실행이 될 때마다 Native Code로 변환하여 실행 속도를 개선했다. 하지만 비용이 발생하기 때문에 먼저 Interpreter 방식을 사용하고, 일정 기준의 시간이 지나면 JIT 방식을 사용하여 실행한다.<br/><br/>

.<br/>
.<br/>
.<br/>
.<br/>
.<br/><br/>

이렇게 해석된 코드들은 Java Runtime Data Area에 공간을 할당받고 수행되며 GC(Garbage Collection)가 작동하면서 삭제되기도 하면서 스레드 동기화가 이루어진다. 사진을 보면 외에 포스팅에서 다루지 않은 과정도 있는데 오늘 포스팅은 JVM이 핵심이므로 이 포스팅에서는 언급하지 않을 것이다. ~~(GC도 나중에 따로 포스팅둘 것이다.)~~  

*스레드 동기화란?*
두 개 이상의 스레드가 동시에 같은 데이터에 접근하게 되면 예상하지 못한 결과가 나올 수도 있는데, 이러한 문제를 방지하는 것이 스레드 동기화다.<br/><br/>


# JRE
> JRE(Java Runtime Environment) : JVM이 실행될 수 있는 환경으로 각 구성 요소들의 활동을 조율한다.

JRE는 자바 프로그램 실행을 위한 라이브러리와 소프트웨어가 포함되어있다. 하지만 Java Programming을 하기위한 도구는 없으므로 <u>읽기만</u> 가능하다. 앞서 Class Loader가 Java 실행 과정에서 런타임 환경의 일부 구성 요소였던 것처럼 JVM 역시 JRE에 포함된다.<br/><br/>


# JDK
> JDK(Java Development Kit): 자바 애플리케이션을 개발하기 위한 핵심 구성요소로 대표적으로 Java Compiler가 있다.

JDK는 상단의 JRE와 프로그래밍 도구가 포함되어있다. JRE는 자바 애플리케이션을 실행만 할 수 있는 반면 JDK는 자바 애플리케이션 개발할 수 있는 도구도 포함하고 있으므로 JDK에 JRE가 포함된다고 볼 수 있다.<br/><br/>

<hr/>

**글을 마치며...**
Java 런타임 환경에서의 구조가 이해하기 힘들었다. 
특히 PC Register의 필요성과 역할이 잘 와닿지 않아서 이리저리 찾아보며 꽤 헤맨 것 같은데 아마도  CPU의 Register에 대한 지식이 없었던 터라 그랬던 것 같다. 컴퓨터 구조 공부를 조금씩 병행해야하나 싶었다.
참고할 자료들을 추가적으로 찾아보는 과정에서도 아직 완벽하게 이해하지 못한 개념적인 부분들(GC, Heap)이 반복적으로 언급이 돼서 추가적으로 복습하고 정리해서 따로 포스팅하면 좋을 것 같다.<br/><br/>

[참고자료]
<br/>
* [바이트코드](https://ko.wikipedia.org/wiki/%EB%B0%94%EC%9D%B4%ED%8A%B8%EC%BD%94%EB%93%9C)
* [백기선님 자바스터디 1주차 : JVM이란, 어떻게 동작하나](https://velog.io/@bongf/study-java-whiteship-javaStudy-week1)
* [[Java] 자바 가상머신 JVM(Java Virtual Machine) 총정리](https://coding-factory.tistory.com/827)
* [[Java] 자바 JVM 내부 구조와 메모리 구조에 대하여](https://coding-factory.tistory.com/828)
* [Native Code와 Managed Code란?](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ehgml1644&logNo=220930953263)
* [JVM의 Runtime Data Area](https://www.holaxprogramming.com/2013/07/16/java-jvm-runtime-data-area/)
* [JAVA - JNI 사용하기](https://mommoo.tistory.com/71)
* [[Java] 자바 스레드 동기화 개념 정리 및 활용 (Thread Synchronization)](https://ittrue.tistory.com/173)
* [ORACLE-Chapter 2. The Structure of the Java Virtual Machine](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html)
* ["JDK란 무엇인가" 자바 개발 키트 소개와 설치하기](https://www.itworld.co.kr/news/110817)

<!-- Back to [전문가를 위한 C++ (Professional C++) 작성 포스트 모음](professional-cpp-toc){:.heading.flip-title}
{:.read-more} -->