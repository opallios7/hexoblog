---
title: 지킬(jekyll)로 깃허브에 블로그 만들기
date: 2017-09-28 19:33:11
tags:
  - 지킬
  - 마크다운
  - 깃헙
  - 깃헙페이지
  - 블로그
  - jekyll
  - markdown
  - mark2html
  - blog
  - github
  - github-pages
categories:
  - 블로그(blog)
  - 지킬(jekyll)
---

지킬과 Github을 연동하여 블로그를 만들수 있다는 소식을 뒤늦게 알고 지킬을 설치하고 Github에 블로그를 만들어보자고 마음을 먹었다. 열심히 구글링을 했는데 대부분 맥을 기준으로 설명한 사이트들이 많았다. 나는 윈도우 유저이기에 윈도우를 기준으로 설명한 사이트가 없나? 하는 생각에 이 글을 작성한다. 다음은 지킬을 설치하는데 도움이 된 사이트 링크이다.

* [Jekyll 윈도우에 설치해서 사용하기](http://tech.whatap.io/2015/09/11/install-jekyll-on-windows/)

### 지킬이 뭐지?
[Jekyll](http://http://jekyllrb.com/)은 마크다운 형태의 텍스트와 레이아웃, 테마등을 정적 HTML로 제너레이트하는 툴이다. Ruby를 기반으로 만들어져 있지만 Ruby의 문법을 알 필요가 없다. (처음 설치시 간단한 명령어 정도만 사용한다.) 또한, HTML과 CSS의 사용도 가능하기에 약간의 지식만 있으면 다양한 모습으로 꾸밀수 있는 장점이 있다.

### 이제 설치해 보자
내가 사용하면서 설치했던 것들은 다음과 같다.
* 루비 ([Ruby](http://rubyinstaller.org/downloads/), [RubyDevkit](http://rubyinstaller.org/downloads/))
* 파이선([Python](https://www.python.org/downloads/))
* Jekyll
* Rouge
* Pygment
* wdm
* bundler
* bundler
* jekyll-feed
* minima

### 루비(Ruby) 설치
Jekyll은 루비(Ruby)기반으로 동작한다. 그래서 Ruby가 반드시 필요하다. 루비와 함께 Development Kit도 함께 설치해야 정상적으로 작동한다.

> 다운로드 사이트
>  [http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/)

위의 사이트에 가면 루비 설치파일이 보일것이다. 본인의 환경에 맞는 것을 다운로드 받아 설치한다.

<p><img src="/assets/Images/blog/jekyll/ruby_download.png" alt="루비 설치파일 다운로드" title="루비 설치파일 다운로드"/></p>

나는 64비트 컴퓨터를 사용하고 있기에 64bit 버전을 다운로드 받아 설치한다.
<p><img src="/assets/Images/blog/jekyll/ruby_install_option.PNG" alt="루비 설치옵션 선택" title="루비 설치옵션 선택"/></p>

설치할때 위의 옵션을 체크하면 어느 경로에서든 루비를 실행할수 있다.

### 루비(Ruby) Development Kit 설치
Development Kit 역시 루비와 같은 경로에 가서 하단으로 조금 내려가면 Development Kit 설치 파일이 보일것이다.

> 다운로드 사이트
>  [http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/)

<p><img src="/assets/Images/blog/jekyll/ruby_developmentkit_download.png" alt="루비 Development Kit설치파일 다운로드" title="루비 Development Kit설치파일 다운로드"/></p>
다운받은 파일을 원하는 경로(나의 경우 "C:\Ruby23-x64"를 사용함)에 복사하고 해당 경로에서 실행하여 설치한다.
설치가 끝나면 DevKit을 사용할 수 있도록 초기화 해야합니다. 윈도우 CMD를 실행하여 DevKit이 설치된 경로로 이동한다.

> ```
> cd C:\Ruby23-x64
>
> ruby dk.rb init
>
> ruby dk.rb install
> ```

### 지킬(Jekyll) 설치

루비 설치가 정상적으로 되었다면 지킬은 설치가 간단하다. 루비의 gem 패키지 인스톨러를 사용하면 된다. 명령어는 다음과 같다.

> ```
> gem install jekyll
> ```

윈도우 권한체크 기능인 UAC가 켜있다면 권한을 물어본다. 설치를 위해서 어드민으로 권한 상승이 필요하니 권한 상승을 줘야 한다.

코드 블럭을 사용하기 위해 Rouge를 설치하는데 이것도 지킬과 같은 방법으로 설치한다.

>  ```
>  gem install rouge
>  ```

여기까지 설치하면 지킬을 사용하기 위한 기본 환경 설치는 끝났다. 다음은 GitHub연동과 마크다운 부가기능을 위한 설치과정을 진행한다.

### 파이썬(Python) 설치
지킬에서 Syntax highlighter를 사용하기 위한 기능이 있는데 이것이 파이썬을 기반으로 작동되기에 먼저 파이썬을 설치한다.

> 다운로드 사이트
>  [https://www.python.org/downloads/](https://www.python.org/downloads/)

<p><img src="/assets/Images/blog/jekyll/python_download.png" alt="파이썬 설치파일 다운로드" title="파이썬 설치파일 다운로드"/></p>

최신버전인 3.5.2을 다운받아 사용했다.

### 파이썬 환경변수에 등록
파이썬 설치가 완료되면 파이썬을 루비처럼 어느 경로에서나 사용이 가능하도록 환경변수에 파이썬이 설치된 경로(Path)를 추가한다. 나는 C:\Python\Python35-32 여기에 설치했다.

> ```
> C:\Python\Python35-32;
> C:\Python\Python35-32\Scripts;
> ```

파이썬이 정상적으로 설치가 되고, 환경변수를 정상적으로 등록되었으면 다음과 같은 화면이 나온다.

<p><img src="/assets/Images/blog/jekyll/python_complete.png" alt="파이썬 설치완료" title="파이썬 설치완료"/></p>

### Pygments 설치
syntax highlighting을 사용하기 위해서 Pygments를 설치해야 한다. 파이썬이 설치되어 있으면 PIP를 이용하여 간단히 설치가 된다.명령어는 다음과 같다.

> ```
> pip install Pygments
> ```

### 지킬(Jekyll) 실행
이제 지킬과 관련된 모든 설치는 끝났다고 생각해도 된다. 그럼 지킬 서비스 구동을 위해 프로젝트 폴더를 하나 만들어보자. 만약 지킬을 구동시키는 데 라이브러리가 없다고 나오면 해당 라이브러리를 gem install을 통해서 설치하고 다시 실행하면 된다. 다음 명령어를 이용하여 프로젝트 디렉토리를 만들어보자.

> ```
>  jekyll new project_name
>  ```

만들어진 디렉토리로 들어가서 지킬을 실행하자.
명령어는 다음과 같다.

> ```
>  cd project_name
>  jekyll serve
>  ```

지킬을 실행시키면 다음과 비슷한 화면이 보이고 하단에 접속 주소 및 포트가 보일것이다. 접속포트는 4000번이고, 서비스 종료는 Ctrl-C를 누르라고 친절하게 설명되어있다.

<p><img src="/assets/Images/blog/jekyll/jekyll_serve.png" alt="지킬 서비스 시작" title="지킬 서비스 시작"/></p>

서비스가 시작되었으면 브라우저를 실행하고 localhost:4000을 실행하면 다음과 같은 화면이 보인다.

<p><img src="/assets/Images/blog/jekyll/jekyll_local_homepage.png" alt="지킬 로컬서비스 홈화면" title="지킬 로컬서비스 홈화면"/></p>

여기까지 지킬 사용을 위한 포스트를 마친다. 다음에는 지킬과 깃을 연동하여 깔끔한 주소(URL)의 개인 블로그를 만드는 방법에 대해서 포스팅 하겠다.
