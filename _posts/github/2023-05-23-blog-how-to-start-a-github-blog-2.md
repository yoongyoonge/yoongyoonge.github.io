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


0. jekyll 적용하기 전 준비 <br>

- jekyll은 gem을 사용해서 설치할 수 있습니다.

#### jekyll이란?
> 일반 텍스트를 정적 사이트로 변환해주는 도구

### gem이란?
> ruby에서 지원하는 패키지 도구 

<br>
로컬 터미널에서 다음의 명령어를 임력하여 jekyll을 실행할 준비를 합니다. <br>

```
$ sudo gem install bundler
$ sudo gem install jekyll
$ cd {githubBlog repository 폴더}
$ bundle exec jekyll serve
```


<br><br> 참고 사이트 <br>
[jekyll][jekylllink]

[jekylllink]: https://jekyllrb.com/

[gem][gemlink]

[gemlink]: https://guides.rubygems.org/what-is-a-gem/

[bundler][bundlerlink]

[bundlerlink]: https://bundler.io/