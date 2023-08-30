---
title: window 컴퓨터에서 linux 환경 사용하기, vscode에서 linux환경을 사용하기
author: yoongyoonge
date: 2023-08-19 20:30:00 +0800
categories: [Blogging]
description: >
    window 컴퓨터에서 linux 환경을 사용하고 싶은데.. 방법이 없을까?
---

~~~
본 포스트는 mac 환경에서만 작업하다가 window 데스크탑을 마련한 사람이 적응해나가는 모습을 담은 포스트입니다.
~~~

<br>

Window 환경에서 Linux 환경을 사용하기 위해서는 WSL(Windowns Subsystem for Linux)을 설치해야 한다.

<br>


1. 가상 머신 플랫폼 활성화 하기
    
    제어판 > 프로그램 > 프로그램 및 기능 > Windown 기능 켜기/끄기 > 가상 머신 플랫폼 활성화 후 재부팅

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/0b5552db-e5f1-4fbb-a1f3-a403d67480e5)


2. wsl 설치할 수 있는 os 확인 후 마음에 드는 os 설치하기

    설치할 수 있는 os 확인

    ```
    wsl --list --online
    ```

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/fce78205-1673-4e40-ac79-9ab4edfc1a36) <br>

    마음에 드는 os 설치 

    ```
    wsl --install -d Ubuntu
    ```

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/0ed55675-c0be-4723-9341-5894c34d3073) <br>

    
    Ubuntu가 설치되었는데 오류가 난다면?

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/66500f54-b692-4eb1-aef0-2ba5cdfcfb74)

    >WSL2버전에서 Linux 커널 업데이트가 되지 않아서 나오는 오류! <br>
    >아래 링크에 가서 각 머신 버전에 맞는 커널 업데이트 패키지를 다운받고 설치해주자!

    커널 업데이트 패키지 다운 링크: 
    https://learn.microsoft.com/ko-kr/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package

    <br>

    그럼 정상적으로 버전이 보인다!

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/78e466f6-cf69-46b1-904a-0e0783c12857)


    그 다음, powershell의 탭에서 아래 화살표를 클릭하면 Ubuntu가 나오는데 이를 클릭하면 아래의 화면을 볼 수 있다

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/e0adf2b6-a05e-4960-9663-334b611817e8)

    
    <span style="color:orange;font-size:20px">그럼 우분투 설치 완료!</span>


3. VSCode 에서 Ubuntu 사용하기

    이제 이 환경을 vscode에서도 사용할 수 있도록 하게 해보려면!

    vscode 코드를 열고 WSL extension을 설치해주고

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/2327bf3e-be50-4182-a441-01480fd09747)


    왼쪽 remote explorer > Ubuntu 아이콘에 커서를 가져가면 화살표가 나오는데 <br>
    Connect in Current Window로 이를 클릭하면 바로 VScode가 우분투에서 실행되는 것처럼 화면이 바뀌면서 Ubuntu에서 VScode를 사용하는 것처럼 환경을 변경할 수 있다!

    ![image](https://github.com/yoongyoonge/yoongyoonge.github.io/assets/20895661/9d9c82ed-6545-4166-8cbd-efc3726d6516)


참고: https://velog.io/@gidskql6671/WSL-WSL2-%EC%84%A4%EC%B9%98-VSCode-%EC%97%B0%EB%8F%99