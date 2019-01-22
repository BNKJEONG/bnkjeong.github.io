---
layout: post
title: "[GIT] 기본 명령어들"
category: GIT
permalink: /git/:year/:month/:day/:title/
tags: [git, add, commit ,push, branch]
comments: true
---

## 변경사항 확인

- 로그에서 출력되는 버전 간의 차이점을 알고 싶을 때

```bash
# 소스 상의 차이점 확인 가능
# -가 이전버전, +가 최신버전
git log -p 
```

- 버전 간의 차이점을 비교할 때

```bash
git diff '버전 id1'..'버전 id2'
```

- git add 하기 전과 add 한 후에 수정을 한 경우, 비교할 때

```bash
git diff
```

## 과거의 버전으로 돌아가기

- 버전 id로 돌아가기

```bash
# 돌아가고 싶은 곳으로 돌아가며 이후의 이력도 지우지 않고 해당 인덱스도 남아있다.
git reset '버전 id' --soft
# 돌아가고 싶은 곳으로 돌아가며 이후의 이력도 지워지지 않지만 인덱스도 초기화 된다.
git reset '버전 id' --mixed # 기본 동작
# 돌아가고 싶은 곳으로 돌아가며 이후의 이력을 모두 지우고 인덱스를 초기화 한다.
git reset '버전 id' --hard
```

- HEAD 기준 리셋

```bash
git reset HEAD~N # 현재부터 N개 이전의 이력으로 돌아가라고 지정
# 바로 이전 커밋으로
git reset HEAD^ --soft # 커밋 취소하되 코드는 살린다.
git reset HEAD^ --hard # 커밋 취소하고 코드도 이전으로 돌아감.
```

- 버전 id를 가진 커밋을취소하면서 새로운 버전 만들기

```bash
git revert '버전 id'
```

- 커밋 메시지 수정하기

```bash
git commit --amend
```

## 브랜치

- 브랜치 목록 볼 때 (커밋하지 않은 저장소일 경우 master가 나타나지 않음)

```bash
git branch
```

- 브랜치 생성할 때

```bash
git branch '브랜치 이름'
```

- 브랜치를 삭제할 때

```bash
git branch -d '브랜치 이름'
```

- 병합하지 않은 상태에서 브랜치 강제 삭제할 때

```bash
git branch -D '브랜치 이름'
```

- 브랜치 전환할 때

```bash
git checkout '브랜치 이름'
```

- 브랜치 생성하고 전환까지 할 때

```bash
git checkout -b '브랜치 이름'
```

- 브랜치 간에 비교할 때

```bash
# 브랜치1에는 없고 브랜치2에 있는 것을 보여줌
git log '브랜치 이름1'..'브랜치 이름2'
```

- 브랜치 간의 코드 비교할 때

```bash
git diff '브랜치 이름1'..'브랜치 이름2'
```

- 브랜치 비교를 좀 더 잘 보고 싶을 때의 옵션들

```bash
# 모든 브랜치 표시
git log --branches
# 그래프로 표시
git log --branches --graph
# 브랜치 명까지 표시
git log --branches --graph --decorate
# 한 줄로 보다 간결하게 표시
git log --branches --graph --decorate --oneline
```

- 브랜치 A로 브랜치 B를 병합할 때

```bash
git checkout A
git merge B
```

## 브랜치 병합하는 2가지 종류

[git-scm](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge-%EC%9D%98-%EA%B8%B0%EC%B4%88)

- Fast Forward (빨리감기)
  - 브랜치를 생성하고 작업을 마쳤는데 master에서 아무런 작업이 되지 않은 경우
  - 커밋을 생성하지 않는다.
- Merge Commit
  - 브랜치를 생성하고 작업을 마쳤는데 master에서 다른 작업을 한 경우
  - 커밋을 생성한다.

## 브랜치 충돌 해결하기

같은 이름의 파일에 대해서 동일한 위치를 수정하게 되면 git이 지원하는 자동 merge가 동작하지 않는다. 따라서 수동으로 충돌을 해결해야 하며 아래와 같이 나타난다. 예를 들어 `function a()`를 `function b()`와 `function c()`로 다른 곳에서 수정했다면,

```bash
<<<<<<<<<<<<< HEAD
function b()
=======
function c()
<<<<<<<<<<<<< exp
```

master 브랜치에서 exp 브랜치의 merge를 시도했으나 충돌이 난 상황이다, `HEAD`라고 되어 있는 부분은 현재 브랜치를 뜻한다. 어쨌든 이런 경우에 해결하기 위해선 최종적으로 어떤 수정 방안을 채택할 것인가로 수정을 해주면 된다. master 브랜치에서 `function b()`로 하기로 결정했다면 그렇게 수정이 되어 merge가 된다. 