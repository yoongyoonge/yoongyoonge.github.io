---
title: github blog - jekyll 테마에서 수식을 표현하자(MathJax) + Markdown 주요 수식
author: yoongyoonge
date: 2023-11-26 21:30:00 +0900
categories: [Github, Blog]
tags: [github, blog, git, jekyll, mathematical expression, mathjax, markdown]
description: >
    jekyll 테마를 활용하는 github 블로그에서 포스트에 수식을 넣기 위해 MathJax를 사용하는 방법을 정리
---

> jekyll 테마를 활용하는 github 블로그에서 포스트에 수식을 넣기 위해 MathJax를 사용하는 방법을 정리합니다. <br>
> 기본 테마는 Chirpy Jekyll Theme를 사용하고 있습니다.

1. 마크다운 엔진 변경
- _config.yml 파일의 내용에 다음을 추가 또는 수정

    ```yml
    # Conversion
    markdown: kramdown
    highlighter: rouge
    lsi: false
    excerpt_separator: "\n\n"
    incremental: false
    ```

2. mathjax_support.html 파일 생성
- _includes 디렉토리에 mathjax_support.html 파일 생성 후 아래 내용 입력

    ```html
    <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        TeX: {
          equationNumbers: {
            autoNumber: "AMS"
          }
        },
        tex2jax: {
        inlineMath: [ ['$', '$'] ],
        displayMath: [ ['$$', '$$'] ],
        processEscapes: true,
      }
    });
    MathJax.Hub.Register.MessageHook("Math Processing Error",function (message) {
          alert("Math Processing Error: "+message[1]);
        });
    MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message) {
          alert("Math Processing Error: "+message[1]);
        });
    </script>
    <script type="text/javascript" async
      src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
    </script>
    ```

    - _layouts/default.html 파일의 head 부분에 아래 내용 삽입

    {% raw %}
    ```html
    {% if page.use_math %}
      {% include mathjax_support.html %}
    {% endif %}
    ```
    {% endraw %}

    - !) 제 블로그의 경우 head.html을 _includes에서 가져오므로 해당 파일에 추가 해주었습니다.
    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/e0b43126-2323-4fa4-b200-fccac629a2ab)
    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/5380a91d-b0fc-4488-a3c7-09db10697e88)


3. YAML front-matter 설정
- 수학식을 표시할 포스트의 front-matter에 use_math: true 적용
- 예시

    ```yml
    ---
    title: "Jekyll Github 블로그에 MathJax로 수학식 표시하기"
    tags:
      - Blog
      - MathJax
      - Jekyll
      - LaTeX
    use_math: true
    ---
    ```

    - 블로그 적용
        - 포스트 작성 <br>
        ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/a5d34ea0-4435-483d-af9b-447bee995231) 

        - 수식 작성 <br>
        ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/b0c3f2a9-19dc-480e-b5c8-37b523506fc1)

        - 출력 결과 <br>
        ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/1c3bc05b-bdab-458c-bb6c-71f2470804ee)

참고자료 <br>
[Jekyll Github 블로그에 MathJax로 수학식 표시하기](https://mkkim85.github.io/blog-apply-mathjax-to-jekyll-and-github-pages/)