---
title: TIL.08) Git & Github
date: 2020-07-27 03:51:33
category: Git
thumbnail: './images/git.png'
draft: false
---

![](./images/git.png)

# Git?

- VCS (버전 관리 시스템)
- 코드 변경 사항 내역 기록 및 관리
- 필요 시 이전 상태로 롤백 기능
- 팀 단위 개발시 체계적이고 효과적인 협업

# Git Basics

## Modified

- 수정 된 파일
- 그러나 Commited 되지 않은 상태의 파일
- 파일을 수정, 생성 하게되면 이 상태가 된다.

## Staged

- 수정 된 파일이 곧 Commit 될 거라고 표시 해놓은 상태
- Commit 하기 전 중간 저장 상태
- 해당 개발이 완전이 완료 되지는 않았으나 중간 저장이 필요할 때 사용
    - 💡완전 개발이 완료 되었을 때 Commit 한다.

> **💡왜 중간 저장이 필요 할까?**
>
추가 수정 사항이 있거나 다시 되돌려야 할때 이미 Commit한 상태라면 Commit history에 남기도 하고 되돌리기 까다롭기 때문에 (되돌리기는 가능은 함) 중간 상태를 저장할 필요가 있다.

## Committed

- 수정 사항들이 git에 저장된 상태
- 이러한 행위를 커밋Commit) 한다고 함

![](https://images.velog.io/images/yongineer1990/post/8b07e8b0-5d91-4dad-8474-635c102275db/image.png)

## Basic Git Commands

### `git init`

- 프로젝트를 `git repository`로 만들기 위해서 사용하는 명령어
    - 💡_Project : 개발하고자 하는 소스코드들이 있는 디렉토리_
- `git repository`가 생성되면 `.git` 폴더가 생성되고 버전관리가 시작된다.

### `git add`

- Modified ➡️Staged 상태 변경
- 새로 추가된 파일을 Staged 상태로 옮길때도 사용
    - 💡_새로 추가된 파일들은 Untracked 파일이라고 함_
- `git add <file name>` : 해당 수정 파일을 Staged 시킴
- `git add .` : 모든 수정된 파일들을 Staged 시킴

### `git commit`

- Stage된 파일을 commit 함
- `git commit -m <msg>` : 커밋하면서 메시지를 남김
    - 💡_메시지를 남기는 이유 : 롤백 시 해당 메시지가 기준이 되기 때문_

### `git diff`

- 어떤 수정 사항들이 적용 됐는지 보고자 할 때 사용
- Staged된 수정 사항을 볼 수 없음
- Modified된 파일만 볼 수 있음

### `git status`

- 현재 상태를 보여주는 명령어
- 어떤 파일들이 Modified 되었고 어떤 파일들이 Staged 되었는지 확인

### `git log`

- Commit 내역들을 보여줌
- Commit history 라고 함
- 기본 포맷이 보기가 쉽지 않아 `tig`같은 tool이 있으면 좋다

### `git rm`

- 원하는 파일을 `git repository`에서 삭제 시킴

### `git mv`

- 원하는 파일을 `git repository`상에서 이동 시킬 때 사용
- 주로 rename할 때 사용

### `git branch`

- Branch를 생성 및 삭제
- `git branch <branch name>` : 브랜치 생성
- `git branch -d <branch name>` : 브랜치 삭제

### `git checkout`

- 어떤 브랜치를 checkout 할때 사용
- 브랜치 이동
- `git checkout <branch name>`

## Branch & Merging

1. Matser branch를 check out 한다.
2. 자신만의 feature branch를 만든다.
3. Feature branch에서 개발을 한다.
4. 완료되면 commit 한다.
5. Master branch에 feature branch를 merge 한다.

![](https://images.velog.io/images/yongineer1990/post/6f514a73-8e98-4b08-a688-f71eb6e3169b/image.png)

## `.gitignore`

- commit 하지 않아도 되는 파일들을 Ignore 처리 하는 것
- `.gitignore` 파일로 관리

## Github?

- git의 중앙 서버 역할
- Code Review
- Documentation 생성 및 관리(
