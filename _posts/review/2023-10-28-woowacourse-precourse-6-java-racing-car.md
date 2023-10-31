---
title: "[우아한 테크코스-프리코스 2주차] git commit 컨벤션"
layout: post
subtitle: "woowacourse precourse 6 - 2nd week mission"
category: review devlog
tags: java woowacourse-precourse-6
published: true
future: true
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}



**[우아한 테크코스 2주차 미션 자동차 경주 게임 GitHub Link](https://github.com/woowacourse-precourse/java-racingcar-6)**

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

### 자주 사용되는 type


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

* 총 글자 수는 50자 이내로 작성
* 마지막에 마침표, 느낌표, 물음표 등의 특수문자는 삽입하지 않음
* 제목은 <U>**개조식 구문**</U>으로 작성

<br/>
<br/>

## body
생략가능하고 **commit 이유, 부연설명** 이 필요한 경우에 작성한다.

* 본문은 **한 줄 당 72자 내로** 작성
* 양에 구애받지 않고 **최대한 상세히** 작성
* 어떻게 변경했는지 보다 **무엇을 변경했는지 또는 왜 변경했는지** 작성

<br/>
<br/>

## footer
생략 가능하고 issue tracker id 를 작성할 때 사용한다. 

* `유형: #이슈 번호` 형식으로 사용
* 이슈 번호가 여러 개 일 때는 쉼표로 구분

### 이슈 유형
> * Fixes: 이슈 수정중 (아직 해결되지 않은 경우)
> * Resolves: 이슈를 해결했을 때 사용
> * Ref: 참고할 이슈가 있을 때 사용
> * Related to: 해당 커밋에 관련된 이슈번호 (아직 해결되지 않은 경우)

<br/>
<br/>

# commit emoji
commit 컨벤션을 지켜야하는 이유는 협업할 때나, 혼자 작업한 것을 확인할 때에도 가독성을 높이기 위해서이다. 이를 위해 **commit message에 emoji**를 사용하면 도움이 된다고 한다. 하지만 이번 미션에서는 기본적인 git commit 양식 지키는 것에 충실하고 emoji는 다음에 사용해볼 예정이다.

<br/>
<br/>

# commit 컨벤션 적용 전후
***

### 적용 전 `hYe807n/hYe807n.github.io`
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/6c9feb22-52f3-4405-9aa3-72555129e3a6)


### 적용 후 `hYe807n/java-racingcar-6`
![image](https://github.com/hYe807n/hYe807n.github.io/assets/79916066/ea9b99a1-3dbf-46c0-a5a3-3c863fd1a157)

<br/>

> #### 장점 
> * 가독성 향상
> * 단위별 commit을 통한 단계별 복구(버그 추적/디버깅)
> * 프로젝트 진행 상황 파악
> * 통일된 양식으로 소통 효율성

<br/>

commit 컨벤션을 지키기 전, `github.io` 블로그를 만들며 작성한 commitm message를 보면 확실히 무엇을 수정한 것인지 눈에 들어오지 않는다. 
*실제로 내가 직접 구현한 블로그 코드 임에도 commit message만 보았을 때는 코드를 수정한 것인지, 삭제한 것인지, 파일을 추가한 것인지도 알기 힘들었다.*
수정 사항을 반영하기 위해 commit 을 올릴 때, *사소한 오타 수정 등의 경우에는 뭐라고 적어야할지 몰라 `"."` 정도로 적었고,* 이 역시 무엇을 위해 message를 작성한 것인지 알아보기 힘들었다.
또한 기능 별로 commit을 작게 나누어서 올리는 것이 아닌, 크게 크게 한참 진행하다가 commit 을 올려서 *전 단계로 복구하기에도 어려웠다.*
하지만 이번 미션을 통해 commit 컨벤션을 지키며 message를 남기니 현재 commit의 내용을 `수정/삭제/리팩토링/기능추가` 등으로 바로 알 수 있었다. 이렇게 단위 별로 나누어서 commit message를 올리니 오류 등의 **문제 상황에 이르러도 전 단계로 돌아가기 훨씬 쉬웠다.** ~~*commit message 컨벤션 적용 전의 프로젝트는 한 단계 전으로 돌아가려 했으면 아마 한참 전으로 돌아갔을 것이다..*~~

<br/>
<br/>

# commit 컨벤션 전후 비교해보니..
우아한 테크코스의 프리코스 미션을 푸는 과정에서 요구 사항이였던 **commit 컨벤션을 지키며 기능 목록 단위로 message 를 남기니 프로젝트를 완성해나갈 수록 눈에 띄게 차이가 비교**가 되었다.
단순히 무엇을 수정했는지 풀어쓰는 것이 아닌, <U>`type: ` 으로 기능 추가를 위한 것인지, 리팩토링을 위한 것인지, 삭제, 수정 등으로 크게 나누어서 작성하여서 눈에 잘 띄었다.</U> `body`, `footer` 는 이번 프로젝트에서는 크게 복잡한 내용이 없다고 판단하게 되어서 생략했지만, 규모가 큰 프로젝트를 진행할 수록 유용하게 사용될 것 같았다.
혼자 작성한 프로젝트의 commit message 였음에도, 정해진 컨벤션을 지키지 않았을 때에는 작성할 때마다 양식이 뒤죽박죽이라 알아보기 힘들었다. 더불어 규모가 더 큰 **협업 프로젝트를 진행할 때엔 더더욱 사람마다 말투가 다르듯이 commit message 양식도 가지각색 일테니 컨벤션을 지키지 않고서는 소통하기 힘들 것**이다.
 또한 commit을 type 별로 나누어 찍기 위해 코드 짜는 과정에서 기능 구현하다가 다른 기능 수정하고, 다른 문서 삭제하는 등의 **번잡한 구현 방식은 지양**하게 되었다. **기능 목록 정리대로 commit을 찍으니 작업과정을 다시 뒤로 되돌리기도 수월**했다.

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
* [Gitmoji 사용하기](https://treasurebear.tistory.com/70)