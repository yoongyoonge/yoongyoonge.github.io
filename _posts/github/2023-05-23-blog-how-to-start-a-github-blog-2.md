---
title: 2. github 블로그 만들기 - 테마 설정 (jekyll) 및 주요 속성 변경 
author: yoongyoonge
date: 2023-05-23 20:30:00 +0800
categories: [Github, Blog]
tags: [github, blog, git, jekyll]
description: >
    jekyll을 이용한 블로그 테마 세팅 과정 / 주요 속성 변경
---

~~~
본 포스트는 mac 환경에서 작업한 내용들을 담았습니다.
~~~


0. rbenv & jekyll 설치 <br>

- jekyll은 gem을 사용해서 설치할 수 있다. jekyll과 bundler를 통해 앞으로 로컬에서 포스팅 결과를 테스트용으로 확인할 수 있도록 할 것인데 이 방법에 대해서 정리하도록 한다.

#### jekyll이란?
> 일반 텍스트를 정적 사이트로 변환해주는 도구

### gem이란?
> ruby에서 지원하는 패키지 도구

### bundler란?
> 정확한 gem과 필요한 버전들을 추적하고 설치하여 ruby 프로젝트에 일관적인 환경을 제공 <br>
> gem 종속성만을 처리하는 RubyGem과 루비 어플리케이션을 위한 환경인 bundler는 이 점이 차이

<br>
로컬 터미널에서 다음의 명령어를 입력하여 jekyll을 실행할 준비를 한다. <br>

```
$ sudo gem install bundler
$ sudo gem install jekyll
```

!) 이 때 각 컴퓨터 환경마다 오류가 발생할 수도 있는데 mac m1 기준에서는 다음의 추가 명령어로 오류들을 해결하였다! <br>
(아래 코드블럭에서 '#' 뒤는 입력하지 않는 부분) <br>

```
##### rbenv 설치 및 환경변수 설정
$ sudo brew install rbenv # 루비설치
$ sudo rbenv install 3.2.2 # 'rbenv install --list' 로 latest stable 버전을 확인한 후 설치하고 싶은 버전을 설치
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc # zsh를 사용하므로 zshrc에 설정
$ echo 'eval "$(rbenv init -)"' >> ~/.zshrc 
$ source ~/.zshrc
$ rbenv # 설치 확인

##### bundler & jekyll 설치
$ sudo gem install bundler
$ sudo gem install rouge -v 3.30.0
$ sudo gem install sass-embedded -v 1.58.3
$ sudo gem intsall jekyll # jekyll 설치
```

<br>

1.  로컬 개발 모드 실행 <br>

```
$ cd {githubBlog repository 폴더} # 클론 레포지토리로 이동
$ jekyll new ./ # 클론 레포지토리에서 jekyll 설치
$ bundle exec jekyll serve # 로컬 개발 모드 실행
```

이후 인터넷 주소창에 <span style="color:orange">127.0.0.1:4000</span> 또는 <span style="color:orange">localhost:4000</span>을 입력하면 테스트를 위한 로컬 개발 모드가 실행되어 페이지가 로딩되는 것을 확인할 수 있게 된다. <br>
+) 별도의 터미널을 열어서 수행해도 되지만 VSCode 등의 IDE로 레포지토리를 작업폴더로 열어 작업하면 창 한 개로 작업할 수 있어 편리합니다!


<br>
2. 테마 입히기 <br>

블로그를 이제 만들었으니 예쁜 옷을 입혀 볼 차례다. <br>
처음부터 꾸며도 되지만 구성요소나 작동 방법 등 아무것도 모르므로 잘 나와있는 테마를 입혀서 수정하도록 하자!<br>

Jekyll을 이용한 테마들을 많은 커뮤니티를 통해 다운로드 받아 입힐 수 있다. <br>

jekyll 테마는 아래와 같은 사이트에서 샘플을 확인 한 후 마음에 드는 테마를 로컬 폴더에 다운로드 시켜두도록 한다. <br>

- http://jekyllthemes.org/ <br>
- https://jekyll-themes.com/free <br>
- http://themes.jekyllrc.org/ <br>

+) 나는 많이 선택하는 테마 중 하나인 'Hydejack'을 다운로드 했다! <br>
http://jekyllthemes.org/themes/hydejack/ <br>

<img width="771" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/561ab66f-b37e-437b-8c05-15704a1a3209"> <br>

위 링크에서는 다운로드를 누르면 테마가 담겨있는 압축 폴더가 바로 다운로드 받아진다. <br>
다른 사이트나 다른 테마가 바로 다운로드가 되지 않아도 해당 테마에 관련한 git 등을 통해 테마 소스 코드가 담겨있는 압축폴더를 다운받아 준비해두면 테마 입힐 준비가 50% 완성된 것이다! <br>

zip 폴더가 준비 되었으면 압축을 풀고 풀어낸 모든 소스코드 들을 앞서 생성해둔 나의 github blog 레포지토리에 그대로 복사하면 테마 입히기 끝! <br>

<p align="center" style="color:gray">
<img width="462" alt="image" title="" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/30b5859a-e527-4cc7-897a-ef6518bfad8f"> <br>
압축 푼 hydejack
</p>

<br>

<p align="center" style="color:gray">
<img width="463" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/a03a3270-6871-4fb9-bd6c-36e258d34460"> <br>
로컬 github blog 개발 폴더에 복사/붙여넣기 한 모습
</p>

<br>

이 상태로 vscode에서 해당 작업 폴더를 열고 터미널을 열어 아래와 같이 입력하면! <br>
bundle로 jekyll을 이용해 로컬에서 테스트 웹이 구동된다!
<img width="500" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/a9cab842-9ebe-45ac-a108-80c247614723"> <br>

<img width="1238" alt="image" src="https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/1a545ec4-b309-4569-8a32-bd86e2ceaba3"> <br>

위 이미지는 github blog를 처음 접하고 나서 테마를 커스터마이징 하느라 hydejack 기본 화면에 주소창만 수정 한 화면인데 아마 <span style="color:orange">127.0.0.1:4000</span> 또는 <span style="color:orange">localhost:4000</span>를 주소창에 입력하면 위 화면과 비슷하게 나올 것이다! 

<br>

<p style="color:gray; font-size : 14px"> 메인 화면은 목록이 나왔던거같은데 사이드바는 저 화면이었던 것으로 기억...</p>

<br>

그럼 테마입히기까지 완성! <br>
다음 포스트는 테마를 나를 나타내는 블로그로 커스터마이징 하는 것을 작성해 볼 것이다! <br>
<br>
<br>


<span style="color:orange">**!!) 2023-08-31 업데이트**</span> <br>
유지보수가 너무 번거로워서 좀 더 관리가 쉬운 테마로 변경했다! <br>
변경한 테마는 [jekyll-theme-chirpy][jekyll-theme-chirpy]

[jekyll-theme-chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy



<br><br> 
참고 사이트 
<br>

[jekyll](https://jekyllrb.com/)

[gem](https://guides.rubygems.org/what-is-a-gem/)

[bundler](https://bundler.io/)

[Jekyll 블로그 시작하기](https://nachwon.github.io/jekyllblog/)