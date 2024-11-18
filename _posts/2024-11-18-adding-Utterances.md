---
layout: post
title: 댓글을 달리게 해보자!
author: simsi012
date: 2024-11-18 22:19:00 +0800
categories: [Blog, Making]
tags: [Start, Make, Work]
published: true
---

### Utterances
가장 많이 추천하는 댓글 기능 추가 방법입니다.

## 설치
- [Utterances 홈페이지](https://github.com/apps/utterances)에 들어가서 Configure을 누릅니다.
- 본인의 레포지토리에 하시면 됩니다.
- 여기서 레포지토리가 Public으로 설정되어있어야 합니다.
- repository 이름은 ex: simsi012/simsi012.github.io
- issue에 적용되는 타입은 자유롭게 하시면 됩니다. 저는 pathname 선택했습니다.
- Theme타입도 개인의 선호대로 하시면 됩니다.
- 이렇게 하시면 html 코드가 나옵니다.

밑에 보시는 것과 같이 나옵니다.

```html
    <script src="https://utteranc.es/client.js"
        repo="simsi012/simsi012.github.io"
        issue-term="pathname"
        theme="photon-dark"
        crossorigin="anonymous"
        async>
    </script>
```

그리고 위의 코드를 chirpy 테마에서는
```shell
_layouts/post.html
```
에 넣어줍니다. 여기서 원하는 부분에 넣어주면 되는데 저는 밑에 코드처럼 넣었습니다.

{% raw %}
```html
<div class="post-tail-wrapper text-muted">
    <!-- categories -->
    {% if page.categories.size > 0 %}
      <div class="post-meta mb-3">
        <i class="far fa-folder-open fa-fw me-1"></i>
        {% for category in page.categories %}
          <a href="{{ site.baseurl }}/categories/{{ category | slugify | url_encode }}/">{{ category }}</a>
          {%- unless forloop.last -%},{%- endunless -%}
        {% endfor %}
      </div>
    {% endif %}

    <!-- tags -->
    {% if page.tags.size > 0 %}
      <div class="post-tags">
        <i class="fa fa-tags fa-fw me-1"></i>
        {% for tag in page.tags %}
          <a
            href="{{ site.baseurl }}/tags/{{ tag | slugify | url_encode }}/"
            class="post-tag no-text-decoration"
          >
            {{- tag -}}
          </a>
        {% endfor %}
      </div>
    {% endif %}

    <div
      class="
        post-tail-bottom
        d-flex justify-content-between align-items-center mt-5 pb-2
      "
    >
      <div class="license-wrapper">
        {% if site.data.locales[lang].copyright.license.template %}
          {% capture _replacement %}
        <a href="{{ site.data.locales[lang].copyright.license.link }}">
          {{ site.data.locales[lang].copyright.license.name }}
        </a>
        {% endcapture %}

          {{ site.data.locales[lang].copyright.license.template | replace: ':LICENSE_NAME', _replacement }}
        {% endif %}
      </div>

      {% include post-sharing.html lang=lang %}
    </div>

    <script src="https://utteranc.es/client.js"
        repo="simsi012/simsi012.github.io"
        issue-term="pathname"
        theme="photon-dark"
        crossorigin="anonymous"
        async>
    </script>

    <!-- .post-tail-bottom -->
```
{% endraw %}

그러면 지금 페이지처럼 밑에 댓글창이 나오게 됩니다.



