---
layout: post
title: 블로그 제작기.1
description: >-
  레포지토리 만들기 부터 테마 적용 그리고 포스팅
author: simsi012
date: 2024-11-13 23:19:00 +0800
categories: [Blog, Making]
tags: [Start, Make, Work]
published: true
---

---

준비물
-------------

● git, github, Visual Studio, Ruby, node.js

사전 기본 세팅
-------------
1. Visual Studio로 git을 쓸 수 있어야 합니다.
2. ruby 마지막 설치 중 [1, 3] 이 중 입력 시 1로 해주세요. (이유는 추후 설명)

본격적인 제작기.
---------------
저는 우선 학교 교수님께서 만드신 방법을 그대로 했습니다.

https://github.com/SKHU-OSS-2024-2/information/blob/main/blog.md

이대로 따라한 후에 post를 업데이트 했습니다. 
(밑의 가이드라인의 저작권은 위의 링크에 올리신 교수님께 있습니다.)

# Github Page 기반 블로그 제작 가이드

## Step 0. 사전 준비사항

- Windows 64bit
- Git 설치
- VS Code 설치
- Github 가입

## Step 1. Github Page 생성

### Step 1-1. Repository 생성

- **github username**이 `karina`인 경우, **repository name**을 `karina.github.io`로 설정
- **Public** 체크
- **Add a README file** 체크

### Step 1-2. Github Page 설정

- 생성한 리포지토리로 이동, 상단 **Settings** 클릭
- 좌측 **Pages** 클릭
- **Source**를 `Deploy from a branch`로 설정
- 사이트 접속 확인 (예시: `https://karina.github.io`)

### Step 1-3. VS Code 활용

#### 리포지토리 클론
- VS Code 열기 >> F1 키 입력 >> git clone 검색 >> Git: Clone 선택
- 리포지토리 주소 입력 >> 클론할 위치 선택
- 이 때, 한글이 포함된 경로를 선택하지 않도록 주의

#### 로컬 변경사항 적용
- 클론한 리포지토리 열기 (`README.md` 파일 확인)
- `index.html` 파일 생성
```html
<html>
	<body>
		Hello! This is the first page!
	</body>
</html>
```
- 좌측 **Source Control** 메뉴 선택
- `+` 버튼을 클릭하여 변경사항 추가
- 커밋 메시지 입력, 커밋 & 푸시
- 사이트 반영 확인 (예시: `https://karina.github.io`)

## Step 2. 로컬 개발 환경 구축

### Step 2-1. Ruby 설치

- [공식 홈페이지](https://rubyinstaller.org/downloads/)에서 최신버전 다운로드 (Ruby+Devkit x.y.z-1 (x64)) 및 설치
- 시작(윈도우 키)메뉴에서 `Start Command Prompt with Ruby` 실행
- cd 명령어로 리포지토리가 있는 위치로 이동
```shell
cd C:\Users\karina\Documents\karina.github.io
```
- jekyll, bundler, webrick 설치
```shell
gem install jekyll bundler
gem install webrick
```
- 설치 확인
```shell
ruby -v
jekyll -v
bundler -v
```

### Step 2-2. Jekyll 서버 구축

- jekyll 생성
```shell
jekyll new ./
# 또는
jekyll new ./ --force
```
- bundle install
```shell
bundle install
```
- jekyll 서버 실행
```shell
bundle exec jekyll serve
```
- http://127.0.0.1:4000/ 또는 http://localhost:4000/ 접속 확인

## Step 3. Jekyll 테마 적용

### Step 3-0. 테마 선택
- http://jekyllthemes.org
- https://jekyllthemes.io/free
- https://themes.jekyllrc.org
- https://github.com/topics/jekyll-theme

### Step 3-1. chirpy 테마 적용
- [공식 홈페이지](https://github.com/cotes2020/jekyll-theme-chirpy)에서 압축파일 다운로드
- 압축을 해제한 뒤, 모든 파일과 폴더를 로컬 리포지토리로 복사
- bundle install
```shell
bundle install
```
- jekyll 서버 실행
```shell
bundle exec jekyll serve
```
- http://127.0.0.1:4000/ 또는 http://localhost:4000/ 접속 확인
- 모든 변경사항 커밋 및 푸시


### Step 3-2. Github Action 적용
- 원격 리포지토리의 상단 **Settings** 클릭
- 좌측 **Pages** 클릭
- **Source**를 `Github Actions`로 설정
- `Configure` 클릭
- `Commit changes...` 클릭
- 로컬 리포지토리에서 pull

### Step 3-3. Node.js 설치
[공식 홈페이지](https://nodejs.org/en/)에서 최신버전 다운로드 및 설치
- Ruby 프롬프트에서 아래 명령어 실행
```shell
npm install && npm run build
```

### Step 3-4. 테마 상세 설정
- `.gitignore` 파일 하단 수정
```shell
# Misc
# _sass/dist
# assets/js/dist
```

- `_config.yml` 파일 수정

```shell
timezone: Asia/Seoul

url: "https://karina.github.io"

github: username: karina
```

- 모든 변경사항 커밋 및 푸시
- 커밋 메시지 주의
- 사이트 반영 확인 (예시: `https://karina.github.io`)


### Step 3-5. 테마 오류 수정
- 테마 오류 중 크게 2가지가 있었습니다.
1. 포스팅을 했는데 화면에 안뜰 때.
2. 화면 홈화면에 작성한 포스팅이 안보일 때.

- 포스팅을 했는데 화면에 안뜰 때.

```shell
published: True
```

위의 코드를 포스트의 메타 데이터에 넣어줍니다.


![publish_code_ex](https://raw.githubusercontent.com/simsi012/simsi012.github.io/refs/heads/main/assets/img/publish.png)


- 화면 홈화면에 작성한 포스팅이 안보일때
  저는 chat.gpt의 도움을 받아서 html코드를 수정했습니다.

```shell
    _layouts/home.html
```

위 경로에 있는 html 코드를 밑에 코드로 해당 부분에 넣어서 해결했습니다.

{% raw %}
```html
<div id="post-list" class="flex-grow-1 px-xl-1">
  {% for post in site.posts %}
    <article class="card-wrapper card">
      <a href="{{ post.url | relative_url }}" class="post-preview row g-0 flex-md-row-reverse">
        {% assign card_body_col = '12' %}
        {% if post.image %}
          {% assign src = post.image.path | default: post.image %}
          {% unless src contains '//' %}
            {% assign src = post.media_subpath | append: '/' | append: src | replace: '//', '/' %}
          {% endunless %}
          {% assign alt = post.image.alt | xml_escape | default: 'Preview Image' %}

          {% assign lqip = null %}

          {% if post.image.lqip %}
            {% capture lqip %}lqip="{{ post.image.lqip }}"{% endcapture %}
          {% endif %}

          <div class="col-md-5">
            <img src="{{ src }}" alt="{{ alt }}" {{ lqip }}>
          </div>

          {% assign card_body_col = '7' %}
        {% endif %}

        <div class="col-md-{{ card_body_col }}">
          <div class="card-body d-flex flex-column">
            <h1 class="card-title my-2 mt-md-0">{{ post.title }}</h1>
            <div class="card-text content mt-0 mb-3">
              <p>{% include post-description.html %}</p>
            </div>
            <div class="post-meta flex-grow-1 d-flex align-items-end">
              <div class="me-auto">
                <!-- posted date -->
                <i class="far fa-calendar fa-fw me-1"></i>
                {% include datetime.html date=post.date lang=lang %}
                <!-- categories -->
                {% if post.categories.size > 0 %}
                  <i class="far fa-folder-open fa-fw me-1"></i>
                  <span class="categories">
                    {% for category in post.categories %}
                      {{ category }}
                      {%- unless forloop.last -%},{%- endunless -%}
                    {% endfor %}
                  </span>
                {% endif %}
              </div>
              {% if post.pin %}
                <div class="pin ms-1">
                  <i class="fas fa-thumbtack fa-fw"></i>
                  <span>{{ site.data.locales[lang].post.pin_prompt }}</span>
                </div>
              {% endif %}
            </div>
            <!-- .post-meta -->
          </div>
          <!-- .card-body -->
        </div>
      </a>
    </article>
  {% endfor %}
</div>
<!-- #post-list -->
```
{% endraw %}