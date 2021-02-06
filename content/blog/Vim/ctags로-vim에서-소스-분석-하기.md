---
title: ctags로 vim에서 소스 분석 하기
date: 2021-02-07 02:02:85
category: Vim
thumbnail: 'https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Icon-Vim.svg/1200px-Icon-Vim.svg.png'
draft: false
---

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Icon-Vim.svg/1200px-Icon-Vim.svg.png" height="300px" width="300px"></p>

# ctags?

- 소스코드의 함수, 변수, 클래스 등을 `tags`라는 DB로 생성해주는 명령어
- 소스코드 내 변수, 함수, 클래스가 정의된 곳을 찾아가거나 특정 함수를 호출하는 모든 위치를 조회할 수 있다.

# ctags 설치하기

### macOS

```bash
braw install ctags
```

단, macOS에서는 기본적으로 `ctags`가 설치되어 있을 수 있다. `ctags --help` 또는 `ctags --version`명령어를 실행해보면 확인 할 수 있다. 다만, 후술할 `ctags`를 생성하는 명령어인 `ctags -R`과 같은 명령어 사용시 `illegal option --R`과 같은 오류가 발생한다면 재설치가 필요하다.

### linux

```bash
sudo apt-get install ctags
```

## ctags 생성하기

`ctags`는 직접 원하는 파일을 지정해서 생성 할 수 있지만 귀찮기 때문에 `-R`명령어로 모두 생성해준다.

```bash
ctags -R
```

또한 만약 제외시킬 파일들이 있다면 `--exclude`명령을 통해 제외 시킬 수 있다. 그런데 문제는 python 코드에 존재하는 `import`와 `variable`도 모두 `tag`가 생성된다는 점이다. 만약 내가 `class`와 `function`의 정의롤 확인하고 싶지 `variable`나 `import`하는 부분을 확인하고 싶지 않을 경우는 아래와 같은 `--python=kinds=-iv` 옵션을 사용해야 한다. 여기서 `-iv`는 `import`와 `variable`이다.

> **💡 Python kinds 확인하는 방법**
>
>`ctags —list-kinds=python`명령을 통해 추가 및 제거할 수 있는 kinds를 확인 가능하며 그 리스트는 다음과 같다.
>
>```bash
>c  classes
>f  functions
>m  class members
>v  variables
>I  name referring a module defined in other file
>i  modules
>x  name referring a class/variable/function/module defined in other module
>z  function parameters [off]
>l  local variables [off]
>```

문제는 또 있다. 이렇게 생성된 `tag`는 `pip install`을 사용해서 인스톨한 모듈, 그리고 python의 내장 함수들은 `tag`가 생성되지 않는다는 점이다. 만약 내가 인스톨된 모듈 그리고 python의 `library`도 확인하고 싶다면 모듈이 설치되어 있는 경로를 찾아 `tag`를 생성해줘야 한다. 

그런데 일반적으로 python개발환경에서는 가상환경을 사용하는데 이 때문에 가상환경의 경로를 찾아서 `tag`생성해야 한다. 따라서 마지막으로 python 코드를 cmd에서 직접 실행 시켜 `sys.path`를 직접 출력시키고 이를 `ctags` 커맨드와 병합하여 `tag`를 생성하면 된다.

정리하면,

- 내 프로젝트에 모든 소스코드를 분석하고 싶은데,
- 소스분석에 도움이 되지 않는 파일 (`html`, `css`, `.git`등)은 제외 시키고 싶고
- python 코드 분석에 필요하지 않은 `변수`와 `import`도 제외 시키고 싶은데
- 인스톨한 모듈하고 내장함수는 소스분석 하고 싶고
- 근데 이게 가상환경에 있으니 가상환경 경로는 잡아야하는

나의 `ctags` 명령어는 다음과 같다.

```bash
ctags -R --exclude=.git --exclude='*.html' --exclude=node_modules --exclude='*.css' --exclude='*.sql' --exclude='*.csv' --exclude='*.json' --exclude='*.md' -fields=+l --languages=python --python-kinds=-iv -f ./tags . $(python -c "import os, sys; print(' '.join('{}'.format(d) for d in sys.path if os.path.isdir(d)))")
```

# Vim과 연동하기

## tag경로 설정하기

`vim` 에서는 `~/.vimrc` `nvim`에서는 `~./config/nvim/init.vim`을 열어 다음과 같이 경로를 설정해준다.

```bash
set tags = [tag 경로]
```

그런데 문제는 `tags`가 여러 경로에 걸쳐 존재 할 수 있다는 사실이다. 이러한 환경의 문제점은 `vim`에서 `ctags`를 사용하기 위해서는 `tags`가 생성된 경로에서 `vim`을 실행해야 하는데 만약 내가 그 하위 디렉토리 또는 상위 디렉토리에서 `vim`을 실행하게 된다면 tags를 읽을 수 없게된다. 

따라서 내가 현재 위치한 경로 부터 상위, 하위 디렉토리 모두 검색해여 `tags`를 검색해야 하는데 이때 다음과 같이 경로 설정을 해준다.

```bash
set tags +=./tags,tags;
```

## ctags 명령어

### 자주 사용 하는 명령어

- `:tj` or `Ctrl + ]` : 해당 함수, 클래스 등에서 입력하면 해당 함수, 클래스 등이 선언된 위치로 이동한다.
- `:po` or `Ctrl + t` : 이전 태그로 돌아간다.
- `:tag [tag name]` : 입력한 태그로 이동한다. 자동완성이 지원된다.

> **💡 `Ctrl + t`가 너무 불편하다면...**
>
>나는 이전태그로 돌아가는 단축키가 직관적이지도 않고 불편하여 vim의 설정파일을 열고 다음과 같은 옵션을 추가하여 키맵핑을 했다.
>```bash
>nnoremap <C-[> <Esc>:po<CR>
>```

### 더 많은 명령어는

[이곳](https://vim.fandom.com/wiki/Browsing_programs_with_tags#ctags_usage_basic_examples)을 참고하자.

## ctags와 같이 사용하면 좋은 플러그인

### [autotag](https://github.com/craigemery/vim-autotag)

- `ctags`는 자동으로 업데이트 되지 않는다 따라서 소스코드를 업데이트, 새로 생성되었다면 `ctags`도 업데이트 해줘야 하는데 이를 도와주는 플러그인이 바로`autotag`다.
- `autotag`를 사용하면 파일이 저장되는 순간 `ctags`를 업데이트 해준다.

### [tagbar](https://github.com/preservim/tagbar)

- 현재 소스코드에서 `tag`를 정리하여 보여준다.
- 정리된 `tag` 리스트에서 해당 태그로 이동하거나, 해당 클래스 안에 어떤 `tag`가 존재하는지 정리되어 보여준다.
