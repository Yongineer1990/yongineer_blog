---
title: 3년차 Vimmer가 추천하는 플러그인
date: 2023-02-19 06:02:40
category: vim
thumbnail: 'https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Icon-Vim.svg/1200px-Icon-Vim.svg.png'
draft: false
---
<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Icon-Vim.svg/1200px-Icon-Vim.svg.png" height="300px" width="300px"></p>

`Vim`에 관해서 언젠가 한번 얘기를 해야 할거 같은데 그렇다고 `Vim `사용법을 공유하기는 너무 식상한데다가
그건 유튜브나 아니면 [Vimtutor](https://opentutorials.org/module/1185/7908) 한번 돌려 보면 기본적인 사용법은 터득할수 있으니
나는 그동안 `Vim`을 사용하면서 추천하고 자주 사용하는 몇가지 개꿀 플러그인을 소개하고자 한다.

# 들어가기 앞서

일단 나는 일반적으로 흔히 사용하는 [Vim](https://ko.wikipedia.org/wiki/Vim)이 아닌 [Neovim](https://neovim.io/) v0.7.2를 사용한다.
추가적으로 나의 세팅은 [ThePrimeagen](https://www.youtube.com/@ThePrimeagen)님의 세팅을 참고하였다.

# Telescope
![](https://camo.githubusercontent.com/3d59e34d1f406890adf620546d3d97017ce0aacda034b1788c66fa872f192134/68747470733a2f2f692e696d6775722e636f6d2f5454546a6136742e676966)

[Fuzzy Finder (Fzf)](https://github.com/junegunn/fzf)기반의 파일 탐색 플러그인이다. 내가 자주 사용하는 기능은 다음과 같다.
 - 파일명 기반의 검색
 - 파일 내 텍스트 검색 (Live Grep % String Grep)
 - Git file 검색
 - Buffer 검색

설치 방법은 [여기](https://github.com/nvim-telescope/telescope.nvim)를 참고하면 된다.


# Debug Adapter Protocol (DAP)
![](https://user-images.githubusercontent.com/38700/124292938-669a7100-db56-11eb-93b8-77b66994fc8a.gif)

`Vim`의 디버깅 도구이며 다음의 기능을 수행한다.
 - 디버깅할 애플리케이션을 실행
 - 실행 중인 애플리케이션에 연결하고 디버깅을 수행
 - Breaking Point를 설정하고 코드를 단계적으로 실행
 - 애플리케이션 상태 검사

설치 방법은 [여기](https://github.com/mfussenegger/nvim-dap)를 참고하면 된다.

# Language Server Protocol (LSP)
![](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*k_lts7S74kYDP3kjSWKsow.png)
[공식문서](https://neovim.io/doc/user/lsp.html)

`Vim`의 언어 서버 프로토콜을 지원하기 위한 플러그인이다. 일반적인 IDE에서 제공하는 프로그래밍 언어의 대한
 - go-to-definition
 - 레퍼런스 검색
 - 형식지원
 - 자동완성

등을 제공한다. 설치 방법은 [여기](https://github.com/neovim/nvim-lspconfig)를 참고하면 된다.

# Harpoon
![](https://user-images.githubusercontent.com/65544203/159124705-1a597110-3291-4610-a193-3d812ded1733.png)

현재 작업중인 파일을 마킹하고 마킹된 파일 사이를 빠르게 이동이 가능한 생산성 향상 플러그인이다.
자주 접근하는 파일들을 마킹하고 마킹된 파일간 이동은 검색과 같은 (귀찮은)과정을 거치지 않고 빠르게 이동하여 접근을 가능하게 해준다.


설치 방법은 [여기](https://github.com/ThePrimeagen/harpoon)를 참고하면 된다.

# 마지막으로
앞서 소개한 플러그인을 포함한 나의 Vim 세팅을 공유하고자 한다. [여기](https://github.com/Yongineer1990/my-vim-setup)를 참고하면 된다.
다만, 나도 내 세팅을 외부에 공개하는것은 처음이라 `Lua Script`나 세팅값이 다소 지저분 할 순 있다. 이 점을 주의 해주기를 바란다.
