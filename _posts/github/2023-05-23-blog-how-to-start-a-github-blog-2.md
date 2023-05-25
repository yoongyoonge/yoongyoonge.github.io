---
layout: post
title: 2. github 블로그 만들기 - 테마 설정 (jekyll) 및 주요 속성 변경 
categories: [blog]
category: blog
description: >
    jekyll을 이용한 블로그 테마 세팅 과정 / 주요 속성 변경
author: yye
---

~~~
본 포스트는 mac 환경에서 작업한 내용들을 담고 있습니다!
~~~


0. rbenv & jekyll 설치 <br>

- jekyll은 gem을 사용해서 설치할 수 있습니다.

#### jekyll이란?
> 일반 텍스트를 정적 사이트로 변환해주는 도구

### gem이란?
> ruby에서 지원하는 패키지 도구

### bundler란?
> 정확한 gem과 필요한 버전들을 추적하고 설치하여 ruby 프로젝트에 일관적인 환경을 제공 <br>
> gem 종속성만을 처리하는 RubyGem과 루비 어플리케이션을 위한 환경인 bundler는 이 점이 차이

<br>
로컬 터미널에서 다음의 명령어를 입력하여 jekyll을 실행할 준비를 합니다. <br>

```
$ sudo gem install bundler
$ sudo gem install jekyll
```

!) 이 때 각 컴퓨터 환경마다 오류가 발생할 수도 있는데 mac m1 기준에서는 다음의 추가 명령어로 오류들을 해결하였습니다! <br>
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

이후 인터넷 주소창에 <span style="color:orange">127.0.0.1:4000</span> 또는 <span style="color:orange">localhost:4000</span>을 입력하면 테스트를 위한 로컬 개발 모드가 실행되어 페이지가 로딩되는 것을 확인할 수 있다. <br>
+) 별도의 터미널을 열어서 수행해도 되지만 VSCode 등의 IDE로 레포지토리를 작업폴더로 열어 작업하면 창 한 개로 작업할 수 있어 편리하다.



<br><br> 
참고 사이트 
<br>

[jekyll](https://jekyllrb.com/)

[gem](https://guides.rubygems.org/what-is-a-gem/)

[bundler](https://bundler.io/)

[Jekyll 블로그 시작하기](https://nachwon.github.io/jekyllblog/)