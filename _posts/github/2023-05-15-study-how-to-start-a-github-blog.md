---
layout: post
title: 1. github 블로그 만들기 준비 및 환경세팅
categories: [blog]
category: blog
description: >
    github 블로그를 세팅하는 과정을 정리합니다.
author: yye
---

0. 환경 <br>
  PC: MacBook Pro (M1/Intel) <br>
  IDE: VSCode

1. gitblog repository 생성 <br> 
  github blog를 만들기 위해서는 github에 레파지토리부터 생성해야한다. 생성하는 방법은 다음과 같고 아래 이미지를 참고해서 생성하면 된다! <br>

<br>

#### ```Repositories 선택 >> New 클릭 >> Repository name 입력 >> Add a README file 체크 >> Create repository 클릭 >> repository 생성 완료!```

<br>

<img width="1358" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/03a8c223-16d2-43b1-a2b5-ac859aaf8ef7">


<img width="786" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/fa839f10-634d-4ca0-8ed8-5a345321f3c2">

<img width="958" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/fe047e30-be12-466e-9a40-16aa28a029bc">

<br>

2. github blog 접속 <br>
git repository를 만들었으니 github blog를 열어볼 수 있다!<br>
하지만 바로 열 수 있는 것은 아니고 github 레파지토리로부터 호스팅 해주는 github pages을 설정해야한다. <br>

repository >> Settings >> Pages >> Github Pages 를 참고하자!

<img width="1340" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/dc41dea4-3615-4349-826c-b5f8407ff8f8">


(터미널 기준 세팅 방법) <br>
2.1 terminal open 
2.2 $ git clone {git repository 주소}
* git repository 주소 확인 방법
code 클릭 >> https:// ~~ 주소 복사
<img width="974" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/c8a7f4aa-8799-477b-a18c-3edc3119b1dc">
2.3 cd {repository name}
2.4 echo "Hello World" > index.html
2.5 git add --all
2.6 git commit -m "Initial commit"
2.7 git push -u origin main

<img width="1332" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/e2408076-f822-48ed-9593-2e0d0213ae98">

1에서 생성했던 repository name 을 주소창에 입력하면 내가 만든 블로그의 초기 화면이 나온다! <br>







참고 사이트 <br>

[creating-a-github-pages-site][createlink]

[createlink]: https://docs.github.com/ko/pages/getting-started-with-github-pages/creating-a-github-pages-site

[Github Pages][githubpageslink]

[githubpageslink]: https://pages.github.com/