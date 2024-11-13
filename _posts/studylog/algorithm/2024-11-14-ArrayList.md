---
title: "코딩테스트 알고리즘 자료구조 ArrayList 동적 배열 정리"
layout: post
subtitle: 자료구조
category: studylog
tags: algorithm java data-structure
published: true
future: true
---

> **이 장에서 배울 내용**
>
> * ArrayList
> * ArrayList, Array 장단점 비교
> * ArrayList 관련 API

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

### 시작하기에 앞서
[지난 포스트](https://hye807n.github.io/studylog/Array.html)에 이어 이번에는 동적 배열인 `ArrayList` 이다. `ArrayList`의 개념과 정적/동적 배열 둘의 장단점을 비교해볼 것이다.
<br/>
<br/>
<br/>

# ArrayList 동적 배열
정적 배열인 `Array`와 다르게 정적으로 데이터 크기가 가변적이라서 크기 제한이 없다.
기본 크기를 초기화하지 않는다면 default로 `10`으로 설정된다. `capacity` 매개변수를 통해 크기를 초기화해줄 수 있다. 설정된 용량보다 **데이터가 초과되면 기존의 용량 1.5 배 만큼 크기를 증가**시킨다. 
`Array` 와의 공통점은 둘 다 데이터를 메모리 공간에 *연속적*으로 저장한다는 점이다. 다만, `Array` 같은 경우 정해진 크기 내의 공간에서 특정 데이터를 삭제/추가해도 해당 공간이 빈 공간으로 남게 된다. `ArrayList`는 추가/삭제 시 동적으로 변경된 크기에 따라 메모리를 재할당한다.
<br/>
<br/>

### 시간 복잡도
데이터를 마지막에 삽입하거나, `index`를 데이터 통한 접근 시 시간 복잡도는 **O(1)**이다. 하지만 `Array` 와 마찬가지로 *중간에 데이터를 삽입하거나 삭제하는 작업은 역시 뒤의 데이터들을 차례대로 연결해주어야 하기때문에 **O(N)** *이다.

<br/>
<br/>

### ArrayList vs Array

![ArrayList 삽입](https://velog.velcdn.com/images%2Fnnnyeong%2Fpost%2Ff16f66d9-4687-4421-8269-26748f45d6b7%2Fimage.png)
![ArrayList 삭제](https://velog.velcdn.com/images%2Fnnnyeong%2Fpost%2F724e43f2-01fe-4bf5-93f8-d0970f526781%2Fimage.png)
>   - ArrayList 삽입 삭제

- **장점**
    -  **기능적 편리함** : `ArrayList` 는 `add()`, `remove()`처럼 `Array` 보다 더 다양하고 편리한 기능을 더 많이 제공한다. 
    - **가변성** : `ArrayList` 는 `Array` 와 비교하여 크기 조정과 추가/삭제가 자유롭다. 다만, *특정 데이터를 찾는 기능은 `Array` 보다 성능이 떨어질 수도 있다.*
- **단점**
    - **속도(삭제기능)**: `ArrayList` 는 빈 공간을 허용하지 않고, `Array` 는 삭제 시 해당 공간이 빈 공간으로 남는다. 그래서 삭제 작업이 많을 경우에는 `ArrayList` 속도가 더 느리다.
    - **주소를 통한 접근**: `Array`와 다르게 연속적으로 메모리 할당을 받지 않고, 주소로 가지고 있기 때문에 index 를 통한 접근 속도가 더 느리다.
    - **메모리 공간 소모**: `ArrayList` 는 데이터를 객체로 다루기 때문에 적은 양의 데이터 사용 시에는 `Array` 사용이 더 유리하다.</br>
####


<br/>
<br/>

### 관련 메서드
- `add()` : 데이터 추가
- `remove()` : 데이터 삭제
- `delete(T t)`: t 값을 삭제
- `get(int index)`: `index` 를 통하여 요소 확인
- `contains(T t)` : 배열에 t 요소가 있는지 확인하여 true/false 값 반환
- `indexOf(T t)`: 배열에서 t 요소의 `index` 를 `int` 로 반환
- `clear()` : 배열 원소 삭제
- `size()` : 배열 크기 반환
- ` insert(int index, T t)` : 삽입할 요소 `t` 를 `index` 위치에 삽입

<br/>

### 멤버변수
- `private int size` : 배열 크기
- `private T[] elements` : 배열을 저장할 T 타입의 element

<br/>
<br/>
<br/>
.<br/>
.<br/>
.<br/>
.<br/>
.<br/>
<br/>
  
### 결론
배열과 마찬가지로 **요소의 수정 및 삭제가 빈번하지 않지만 순차적인 접근이 가능하거나 조회할 작업이 많을 때에 유용**하다. 또한 다양한 기능이 구현되어있어 편리하고, 정적인 배열이 필요할 때 사용되지만 *데이터 요소의 수정이 적거나, 크기가 정적이라면 `Array`를 사용하는 것이 유리하다.*

<br/>
<br/>
<br/>

[참고자료]<br/> 
-   [[자료구조] 배열(Array) 자료구조 알아보기 & Java 예제 코드(+ ArrayList)](https://hoehen-flug.tistory.com/28)
-	[[자료구조 | Java] 리스트(List) - ArrayList](https://cdragon.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8List-ArrayList)
-	[ArrayList 자료구조 실전 구현 강의 (JAVA)](https://inpa.tistory.com/entry/DS-%F0%9F%A7%B1-ArrayList-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EC%A7%81%EC%A0%91-%EA%B5%AC%ED%98%84-%EA%B0%95%EC%9D%98)
-	[자바 ArrayList 구조 & 사용법 정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-ArrayList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95)
-	[[JAVA]ArrayList의 사용법과 주요 메서드의 시간복잡도](https://chunsubyeong.tistory.com/81)
-	[알고리즘 기본 숙지 내용 : C++와 Java 비교를 중심으로](https://velog.io/@goaldae/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B8%B0%EB%B3%B8-%EC%88%99%EC%A7%80-%EB%82%B4%EC%9A%A9-C%EC%99%80-Java-%EB%B9%84%EA%B5%90%EB%A5%BC-%EC%A4%91%EC%8B%AC%EC%9C%BC%EB%A1%9C)