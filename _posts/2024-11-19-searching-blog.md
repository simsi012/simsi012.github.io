---
layout: post
title: 구글, 네이버에 블로그를 검색했더니 나온다?
author: simsi012
date: 2024-11-18 22:19:00 +0800
categories: [Blog, Making]
tags: [Start, Make, Work]
published: true
---
이번에는 Google Search Console을 이용하여 방문자확인, 통계를 확인해보겠습니다.

### Google Search Console
1. [홈페이지](https://search.google.com/search-console/about)로 들어가서 시작하기를 누릅니다.
2. URL 접두어 칸에 본인의 github 블로그 주소를 입력해주세요. ex: https://simsi012.github.io
3. 입력 후 계속을 누르면 "소유권 확인" 이라는 창과 함께 여러 방법들이 나옵니다.
4. 저는 HTML 파일을 다운 받아서 직접 블로그 루트 디렉토리에 넣은뒤 커밋했습니다.

### Naver Search Console
1. [홈페이지](https://searchadvisor.naver.com/)에 들어가서 네이버 로그인을 해줍니다.
2. 페이지 스크롤을 내리다보면 **웹마스터 도구 사용하기**를 클릭합니다.
3. 사이트 등록에 자신의 github 블로그 url을 넣어줍니다.
4. 소유권 확인 방법은 자유롭게 하시면 됩니다.  
   (저는 블로그 루트 디렉토리에 html 파일을 넣었습니다.)  
5. github에 커밋 후 푸시 해주시면 됩니다.  
6. 그 다음 요청 - 사이트맵 제출 - 사이트맵 URL 입력에서 
   자신의 블로그 주소/sitemap.xml 을 기입해줍니다. (ex: https://simsi012.github.io/sitemap.xml)

### Robot.txt와 sitemap.xml 만들기

1. ## sitemap.xmlt 만들기

```xml
---
layout: null
---

<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd"
        xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    {% for post in site.posts %}
    <url>
        <loc>{{ site.url }}{{ post.url }}</loc>
        {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
        {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
        {% endif %}

        {% if post.sitemap.changefreq == null %}
        <changefreq>weekly</changefreq>
        {% else %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
        {% endif %}

        {% if post.sitemap.priority == null %}
        <priority>0.5</priority>
        {% else %}
        <priority>{{ post.sitemap.priority }}</priority>
        {% endif %}

    </url>
    {% endfor %}
</urlset>
```  
상단 코드를 복사해서 빈 xml 파일 형식에 복사 붙여넣기 해주시면 됩니다.  
마크다운, txt 파일 만드는 것처럼 파일 이름을 sitemap.xml 로 해주시고  
블로그 루트 디렉토리에 넣어주시면 됩니다.  



2. ## Robot.txt 만들기  
  
sitemap.xml과 방법은 동일합니다.  


```txt
User-agent: *
Allow: /

Sitemap: https://kunheelib.github.io/sitemap.xml 
```  
블로그 루트 디렉터리에 Robot.txt 파일을 만들어주고  
위의 코드를 넣어주시면 됩니다.  
  

그리고 며칠 뒤면 밑에 사진처럼 자신의 블로그가 검색 엔진에서 검색됩니다.

![google search](https://github.com/simsi012/simsi012.github.io/blob/main/assets/img/google%20search.png?raw=true)  
  


![naver search](https://github.com/simsi012/simsi012.github.io/blob/main/assets/img/naver%20search.png?raw=true)