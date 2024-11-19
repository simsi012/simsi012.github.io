---
layout: post
title: 방문자를 확인해보자!
author: simsi012
date: 2024-11-18 22:19:00 +0800
categories: [Blog, Making]
tags: [Start, Make, Work]
published: true
---
이번에는 GoatCounter를 이용하여 방문자확인, 통계를 확인해보겠습니다.

### GoatCounter
[홈페이지](https://www.goatcounter.com/)로 들어가서 일단 회원가입을 해줘야합니다.
들어가면 여러 입력칸들이 보일텐데 순서대로 입력해주시면 됩니다.
![Goat Counter sign up](https://raw.githubusercontent.com/simsi012/simsi012.github.io/refs/heads/main/assets/img/goatcounter.png)

## 회원가입

Code : 원하시는 닉네임 쓰시면 됩니다.

Site Domain : 자신의 깃허브 블로그 링크 ex: simsi012.github.io

Email address : 자신의 이메일 주소

Password : 자유롭게 입력하시면 됩니다.

Fill in 9 here : 그냥 9 입력하시면 됩니다.

## 가입 후에는?
맨 위 우측에 Settings가 있습니다.

들어가셔서 "Allow adding visitor counts on your website" 항목 체크하시면 됩니다.

그리고 Save 버튼 누르기.
  
### _config.yml 수정하기

1. _config.yml 파일에서 다음과 같이 수정해주세요.

```yaml
  goatcounter:
    id: 위의 name에 기입한 자신의 코드
```

그리고 provider 부분을 이렇게 수정해주세요.

```yaml
pageviews:
  provider: goatcounter
```

그리고 커밋 후 푸시하시면 됩니다.