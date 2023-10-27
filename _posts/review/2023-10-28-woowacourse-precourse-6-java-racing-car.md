---
title: "[우아한 테크코스-프리코스 2주차] git commit 컨벤션"
layout: post
subtitle: "woowacourse precourse 6 - 2nd week mission"
category: review
tags: java woowacourse-precourse-6
published: true
future: true
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}



**[우아한 테크코스 2주차 미션 숫자 야구 게임 GitHub Link](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)**

# 과제 진행 요구 사항 - git commit 컨벤션
***
![과제 진행 요구 사항](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/6fc8438a-d953-4be7-beb4-a472348ee9cf)

이번 2 주차 미션부터는 Git commit 할 때에 전 단계에서 **정리한 기능 목록 단위로 commit message 컨벤션을 지켜서 작성**해야하는 요구사항이 추가되었다. 미션에서 첨부해준 [커밋 컨벤션 링크](https://gist.github.com/stephenparish/9941e89d80e2bc58a153) 외에도 궁금해서 따로 찾아서 정리해보았다.

<br/>

# commit message 생략
`git bisect skip $(git rev-list --grep irrelevant <good place> HEAD)`

* Formatting change(공백, 빈 줄 추가/제거, 들여쓰기)
* 세미콜론 누락
* 주석
등의 중요하지 않은 commit의 경우 git bisect를 사용하여 중요하지 않은 commit을 무시할 수 있다.

# commit message 구조

기본적으로 commit message는 크게 `제목(subject)`, `본문(body)`, `꼬릿말(footer)` 세 파트로 나눌 수 있다.

```html
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

* 각각의 파트 사이에는 한 줄 공백이 있어야한다.
* `scope`, `body`, `footer`는 생략 가능하다.
* 가독성을 위해 commit message는 100 자를 넘길 수 없다.
>  
> 한글로 작성 가능하다!
>

<br/>
<br/>

## type
commit 을 통해 **변경된 사항의 유형**을 기록한다.

* `type(scope): subject` 형식이며 `:` 뒤에만 공백이 있음에 유의해야한다. 
* 영어로 쓰되, 원형을 사용한다. (ex. fixed(X), removes(X))

### 자주 사용되는 종류


> * feat : 새로운 기능(함수) 추가
> * fix : 버그 수정
> * docs : 문서 수정
> * style : 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
> * refactor : 코드 리펙토링
> * test : 테스트 코드, 리펙토링 테스트 코드 추가
> * chore : 빌드 업무 수정, 패키지 매니저 수정
> * rename : 파일명(or 폴더명) 을 수정한 경우
> * remove : 코드(파일) 의 삭제가 있을 때. ~~"Clean", "Eliminate" 를 사용하기도 함~~

<br/>
<br/>

## subject
코드 변경 사항에 대한 요약을 적는다. 주로 작성한 기능 목록에서의 기능 하나 단위를 적는다.

## subject 규칙
1. 총 글자 수는 50자 이내로 작성
2. 마지막에 마침표, 느낌표, 물음표 등의 특수문자는 삽입하지 않음
3. 제목은 <U>**개조식 구문**</U>으로 작성

<br/>
<br/>

## body
생략가능하고 **commit 이유, 부연설명** 이 필요한 경우에 작성한다.

* 본문은 **한 줄 당 72자 내로** 작성
* 양에 구애받지 않고 **최대한 상세히** 작성
* 어떻게 변경했는지 보다 **무엇을 변경했는지 또는 왜 변경했는지** 작성

## footer
생략 가능하고 issue tracker id 를 작성할 때 사용한다. 

* `유형: #이슈 번호` 형식으로 사용
* 이슈 번호가 여러 개 일 때는 쉼표로 구분

### 이슈 트래커 유형
> * Fixes: 이슈 수정중 (아직 해결되지 않은 경우)
> * Resolves: 이슈를 해결했을 때 사용
> * Ref: 참고할 이슈가 있을 때 사용
> * Related to: 해당 커밋에 관련된 이슈번호 (아직 해결되지 않은 경우)


<br/>
<br/>
<br/>

[참고자료]<br/>
* [Commit Message Conventions](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)
* [[협업] 협업을 위한 git 커밋컨벤션 설정하기](https://overcome-the-limits.tistory.com/entry/%ED%98%91%EC%97%85-%ED%98%91%EC%97%85%EC%9D%84-%EC%9C%84%ED%95%9C-%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9D%B8-git-%EC%BB%A4%EB%B0%8B%EC%BB%A8%EB%B2%A4%EC%85%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)
* [Git | git 커밋 컨벤션 설정하기](https://velog.io/@shin6403/Git-git-%EC%BB%A4%EB%B0%8B-%EC%BB%A8%EB%B2%A4%EC%85%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)
* [[Git] Commit Message Convension (협업을 위한 git 커밋컨벤션)](https://velog.io/@msung99/Git-Commit-Message-Convension)
* [Git 커밋 메세지 컨벤션은 왜 중요할까?](https://yozm.wishket.com/magazine/detail/1974/)
* [[Git/Github] Commit Convention이란?](https://kdjun97.github.io/git-github/commit-convention/)