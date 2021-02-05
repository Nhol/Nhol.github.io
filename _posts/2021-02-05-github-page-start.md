---
title: "GitHub Pages 준비하기"
excerpt: "Ruby, Jekyll 설치 / 샘플 블로그 생성 / 로컬 서버 구동"

categories:
  - Blog
tags:
  - Blog
last_modified_at: 2021-02-05
---


1. ruby, rbenv 설치
    - mac에는 ruby가 default로 설치되어 있으나 버전이 낮아 jekyll 지원하지 않는 문제가 있었음
    - 또한 system ruby가 아닌 rbenv로 관리되는 ruby 설치 필요
        - 다양한 python 및 라이브러리 사용을 위해 venv, pyenv, conda 를 사용하는 것과 유사

    ```bash
    # homebrew update
    $ brew update

    # rbenv 설치
    $ brew install rbenv ruby-build

    # ruby 설치: system ruby가 아닌 rbenv로 관리되는 ruby를 설치하기 위함
    $ rbenv install 3.0.0

    # ruby 글로벌 버전 설정(특정 디렉토리에서 local 버전 설정 가능)
    $ rbenv global 3.0.0

    # rbenv 버전 확인
    $ rbenv versions
    system
    * 3.0.0 (set by /Users/beomyongnho/.rbenv/version) # 이렇게 앞에 *이 붙어야 함
    ```

2. jekyll 설치

    ```bash
    # jekyll 설치 에러
    $ gem install bundler jekyll
    Fetching: bundler-2.2.8.gem (100%)
    ERROR:  While executing gem ... (Gem::FilePermissionError)
        You don't have write permissions for the /Library/Ruby/Gems/2.3.0 directory.
    ```

    ```bash
    # rbenv의 ruby가 실행되도록 .zshrc에 다음 코드 추가 
    $ echo export PATH=$HOME/.rbenv/bin:$PATH && eval "$(rbenv init -)" >> ~/.zshrc
    $ source ~/.zshrc
    ```

    ```bash
    # 성공적
    $ gem install bundler jekyll
    Fetching bundler-2.2.8.gem
    Successfully installed bundler-2.2.8
    ....
    ```

3. jekyll을 이용한 github page 생성(local)

    ```bash
    $ jekyll new username.github.io
    ```

4. local 서버 실행

    ```bash
    # 다음과 같은 에러가 발생
    $ bundle exec jekyll serve
    ...(생략) 
    `require': cannot load such file -- webrick (LoadError) 
    ...(생략)
    ```

    This happens because webrick is no longer a bundled gem in Ruby 3.0. From https://www.ruby-lang.org/en/news/2020/12/25/ruby-3-0-0-released/:

    The following libraries are no longer bundled gems or standard libraries. Install the corresponding gems to use these features.

    sdbm
    webrick
    net-telnet
    xmlrpc

    (참고: [https://github.com/jekyll/jekyll/issues/8523](https://github.com/jekyll/jekyll/issues/8523))

    ```bash
    # 프로젝트의 bundled gem에 webrick 추가 후 서버 다시 실행하면...
    $ bundle add webrick
    $ bundle exec jekyll serve

    Bundler could not find compatible versions for gem "jekyll-sitemap":
      In Gemfile:
        minimal-mistakes-jekyll was resolved to 4.21.0, which depends on
          jekyll-sitemap (~> 1.3)

    Could not find gem 'jekyll-sitemap (~> 1.3)', which is required by gem 'minimal-mistakes-jekyll', in any of the sources.
    ```

    다른 머신에서 생성한 코드를 git clone 하여  실행하는 경우 위와 같은 에러 발생하는 것으로 보임
    이 경우 아래 코드 실행

    ```bash
    $ sudo gem install pygments.rb
    $ gem install bundler
    $ bundle install 
    ```

    → 127.0.0.0:4000(localhost:4000)에서 확인 가능
