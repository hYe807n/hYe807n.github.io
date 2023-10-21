---
title: "[우아한 테크코스-프리코스 1주차] 숫자야구게임 설계"
layout: post
subtitle: "woowacourse precourse 7 - 1st week mission"
category: review
tags: java, woowacourse-precourse
published: true
future: true
---

> **이 장에서 배울 내용**
>


<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

**[우아한 테크코스 1주차 미션 숫자 야구 게임 GitHub](https://github.com/woowacourse-precourse/java-baseball-6)**

# 설계 과정
## 설계? 구현?
프로그램을 구현하기에 앞서, 프로그램 요구사항을 먼저 정확히 파악하기 위해 정리하고 기능 사항을 설계한 후에 구현 단계로 넘어가는 것이 효율적이라는 판단이 들었다. 이 전에 작은 프로젝트를 진행했을 때에는 무턱대고 구현부터 시작해서 수정하는 것에 가장 큰 시간을 소모했던 나의 과거가 생각나며 굉장히 비효율적이였구나 생각이 들었다. 규모가 큰 프로젝트일 수록 구현하기에 앞서 설계가 탄탄해야 중간에 수정하는 과정에서 시간 낭비를 하지 않겠구나..

## 나의 첫 번째 설계
우아한 프리코스에서의 과제 진행 요구 사항은 "`기능을 구현하기 전 docs/README.md에 구현할 기능 목록을 정리해 추가`" 하는 것이였다. 프로그램 설계를 해본 적이 없던 나는, 프로그램을 무작정 만들고 수정하고 수정해가면서 구현하는 방식으로 해왔었는데, 왜 먼저 설계를 한다는 생각을 떠올리지 못했을까? 싶었다. 

## 기능 요구 사항
```md:docs/README.md
## 상대방 컴퓨터
- 3 자리 숫자 문자열을 가짐
- 1 ~ 9까지 서로 다른 임의의 수 3 개 선택 
  - camp.nextstep.edu.missionutils.Randoms - `pickNumberInRange()`
- 플레이어 - 컴퓨터 개수 계산
  - 스트라이크
    - 3스트라이크: 게임 재시작/종료 구분
  - 볼
  - 낫싱

## 입력
- 플레이어가 추측하는 컴퓨터의 서로 다른 3 개의 숫자
    - camp.nextstep.edu.missionutils.Console - `readLine()`
- 게임 재시작/종료를 구분하는 1과 2 중 하나의 수
**잘못된 값 입력 시 `IllegalArgumentException` 발생 후 종료**

## 출력
- 플레이어 - 컴퓨터 개수 계산 출력
- 하나도 없는 경우(낫싱) 경우 출력
- 3개의 숫자를 모두 맞힐 경우(3스트라이크) 경우 출력
- 게임 시작 문구 출력
- 게임 재시작/종료 문구
```

처음으로 정리해본 요구사항은 다음과 같았다. 먼저, 기능 요구 사항, 프로그래밍 요구 사항, 과제 진행 요구 사항을 보고 정리했다. 다음으로 요구사항을 토대로 구체화해서 어떻게 구현할 것인지 "구현 기능 목록"을 설계해서 목록을 정리해보았다. 

## 구현 기능 목록
```md:docs/README.md
# 구현 기능 목록
## Model
- ComputerModel
  - Field 
    - [ ] 중복되지 않은 임의의 숫자 3 개

## View
- InputView
  - Method
    - [ ] 플레이어 수 입력 
    - [ ] 게임 재시작/종료 1과 2중 하나의 수 입력 
    - camp.nextstep.edu.missionutils.Console - `readLine()` 사용
- OutputView
  - Method
    - [ ] 플레이어 - 컴퓨터 점수 출력
    - [ ] 하나도 없는 경우(낫싱) 경우 출력
    - [ ] 3개의 숫자를 모두 맞힐 경우(3스트라이크) 경우 출력
    - [ ] 게임 시작 문구 출력
    - [ ] 게임 재시작/종료 문구 출력 

## Controller
- StateController
  - Field
    - [ ] 현재 게임 진행 상태(1과 2중 하나)
  - Method
    - [ ] 게임 재시작/종료 조정
    - [ ] **잘못된 값 입력 시 `IllegalArgumentException` 발생 후 종료**
- ScoreController
  - Method
    - [ ] 플레이어 - 컴퓨터 점수 계산 

## RandomNumber
  - Method
    - [ ] 중복되지 않은 임의의 숫자값 3 개 반환
    **camp.nextstep.edu.missionutils.Randoms - `pickNumberInRange()`**

```