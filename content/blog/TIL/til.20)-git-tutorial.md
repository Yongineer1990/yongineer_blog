---
title: TIL.20) Git Tutorial
date: 2020-07-27 20:07:33
category: Git 
thumbnail: './images/git.png'
draft: false
---

![](./images/git.png)

# Git repository

## Remote repository
- Remote repository 전용 서버 (Github)에서 관리
- 여러 사람이 공유 하기 위해 사용
- 클라우드와 같음

## Local repository

- 내 PC에서만 저장되는 개인 전용 저장소

평소에는 자신의 Local repository에서 작업 후 작업 완료 시 Remote repository에 업로드한다. 또는 다른 사람들이 작성한 코드를 Remote repository를 통해 자신의 Local repository로 가져 올 수 있다.

## Local repository 만들기

### `git init` 으로 만들기

- 프로젝트 디렉토리에서 `git init` 명령어를 사용한다.

### `git clone`으로 만들기 (Github)

- Remote repository를 Local repository로 복사하는 것이라고 이해 하면 쉽다.
- `git clone <remote repo URL>`

# Staging & Commit

- 파일을 새로 만들거나 기존 파일을 수정한 후 Staging 및 Commit을 하게 된다.

## Staging

**`git add`**
- Staging을 위한 명령어
- `git add <file name>` : 해당 파일을 Staing 한다.
- `git add .` : 변경된 모든 파일을 Staging 한다.

## Commit

**`git commit`**
- commit을 위한 명령어
- `git commit -m <msg>` : Staging된 파일을 Commit 하고 메시지를 남긴다.
- 모든 사항이 commit된 후 `git status` 명령어 사용하여 상태를 확인해 보면 다음과 같은 메시지를 확인 할 수 있다.

```bash
$ git status
# On branch master
nothing to commit (working directory clean)
```
*모든 변경 사항이 commit 됨을 알 수 있다.*
- 💡Working directory = Project의 디렉토리

# Log

- commit history를 확인 할 수 있다.
- `git log` 명령어로 사용한다.

```bash
commit 0656e4eca900ad6b19463f76a10815e724f45ddd (feature/v0.2)
Author: yongineer1990@gmail.com <60580491+Yongineer1990@users.noreply.github.com>
Date:   Thu Jun 4 17:29:03 2020 +0900

    CSV 파일 저장 완료

commit cfa31e1617f69d55c1d2e34a05f4773872838e87
Author: yongineer1990@gmail.com <60580491+Yongineer1990@users.noreply.github.com>
Date:   Thu Jun 4 17:19:43 2020 +0900

    음료 정보 가져오기 완료
```

💡 **`git status` & `git log`**
### `git status`

- 파일들의 상태를 확인
- Untracked 파일과 Tracked 파일들의 상태를 확인 (git의 추적 대상을 확인)
- Tracked 상태라면 `Unmodified` , `modified`, `staged` 상태를 확인

### `git log`

- 해당 Branch의 commit 내역을 보여준다.

# Push & Pull

## Push

- 업로드

### Remote repository에 Push해보기 (Github)

1. **Remote repository의 주소를 이름에 저장하기**
    - `git remote add <name> <remote repo URL>`

    ```bash
    git remote add origin https://github.com/Yongineer1990/Starbucks_Scrapper.git
    ```
    *Github의 Remote repository 경로를 `origin`이라는 이름으로 저장*

    ![](https://images.velog.io/images/yongineer1990/post/18e7cfb7-51a6-4d04-87ee-21db784f31dc/image.png)

    github에선 repository에서 Clone or download 버튼을 클릭하면 주소를 클립보드에 복사 할 수 있다.

2. **Push 하기**
    - `git push <remote> <branch>`
        - 1️⃣에서 주소를 `origin`으로 저장 하였으니 master 브랜치에 Push한다고 가정할 경우
        - `git push origin master`

    💡**Branch명 생략하기**
    - `git push -u origin master`
    *-u 옵션을 사용 하여 한번 branch명을 입력 해주면 이후 Push에서 Branch명을 생략하고 push 할 수 있다.*

## Pull
- 다운로드

    1. **Pull 하기**
        - `git pull <remote> <branch>`
            - 1️⃣에서 주소를 `origin`으로 저장 하였으니 master 브랜치에서 Pull한다고 가정할 경우
            - `git pull origin master`

# Branch

독립적으로 작업을 진행하기 위한 하나의 작업 공간이며 각각의 브랜치는 다른 브랜치에게 영향을 주지 않기 때문에 여러 작업을 동시에 진행 할 수 있다.

## master branch

- repository를 처음 생성할 때 만들어지는 브랜치
- 가장 **안정적인 상태를 유지** 해야한다.
- master 브랜치는 곧 **배포**할 브랜치와 같다.

## topic branch

- 기능 추가나 버그 수정같은 단위 작업을 위한 브랜치

## intergration branch

- 언제든지 배포 할 수 있는 브랜치
- topic branch를 통합 시킨 브랜치

## Branch 전환하기 (Checkout)

- `checkout`명령어를 사용하여 브랜치간 전환한다.
- `git checkout <branch name>`
- 만약 수정 하였으나 `commit`하지 않은 내용이 있는채로 `checkout`하게 되면 그 변경 사항들은 기존 브랜치가 아닌 전환된 브랜치에서 `commit`할 수 있다.
- 만약 변경 내용중이 전환할 브랜치에서도 변경되어 있는 경우 `checkout`은 실패 할 수 있으며 이 경우 변경 내용을 먼저 `commit`하고 브랜치를 전환하거나 `stash`를 사용하여 임시 저장 하여 충돌을 피한 후 `checkout`할 수 있다.

## Branch 통합하기 (Merge)

- `merge`명령어를 사용하여 여러 브랜치를 하나로 병합할 수 있다.
- `git merge <branch name>`
- 이 때 만약 브랜치간 충돌 내역이 존재 할 경우 `Conflict`가 발생하며 이를 해결 후 다시 `commit`해야 올바르게 `merge`할 수 있다.

## Branch 삭제하기

- `git branch -d <branch name>`

💡 **`rebase`**
- `merge`와 다르게 통합하는것이 아닌 통합할 브랜치 뒤로 이동하는 것을 `rebase`라고 한다.
- `git rebase <branch name>`
>
만약 master 에서 새로운 기능 추가를 위한 브랜치 A를 생성하여 작업 하는 것과 동시에 버그 수정을 위한 브랜치 B를 생성하여 버그 수정이 동시에 일어나고 있는 경우 버그 수정이 완료 되어 master 브랜치와 병합 하였지만 새로운 기능 구현을 위한 A브랜치에도 B브랜치의 수정사항이 필요한 상태라고 가정 해보자
>
이때 `rebase`를 이용하면 B브랜치의 수정사항을 반영하여 작업을 이어나갈 수 있게 된다.
