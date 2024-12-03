---
layout: post
title: 우당탕탕 테트리스 게임 개발기_[2]
author: simsi012
date: 2024-11-30 20:45:00 +0800
categories: [Work, Making]
tags: [Make, Work]
published: true
---

***안녕하십니까 여러분***  
  
늘 삶이 일정하게 우당탕탕 흘러가는 주인장입니다.  

오늘도! 테트리스 게임 개발기를 리뷰해보려 합니다.  

저번 시간에는 좌우, 상하 방향키를 동시에 눌렀을 때,  
  
즉 방향키 2개 이상을 동시에 눌렀을 때 블록이 안내려가는 현상을  
고치는 방법을 리뷰 했었는데,  
  
오늘은 좌우, 대각선 움직임이 2칸 이상 가게 되는 버그 수정 리뷰를 해보도록 하겠습니다.

참고로 이거 하느라 엄청 힘들었음 ㅠㅠ
  
원본 코드는 이렇습니다.

```python
 elif event.type == KEYDOWN:
        if event.key == K_LEFT:
            self.board.move_piece(dx=-1, dy=0)  # 즉시 한 칸 이동
            hold_left = True
        elif event.key == K_RIGHT:
            self.board.move_piece(dx=1, dy=0)  # 즉시 한 칸 이동
            hold_right = True

```

얼핏 보면 정상적인 코드 같은데,,, 
막상 실행시켜보면 예상했던 움직임 즉, 대각선으로 한칸 이동이 아닌  
대각선, 양방향으로 두칸 이상 움직이는 모습이 보입니다...  

저는 그동안 코드를 더 넣어야한다고 생각했었는데  
제 옆에 팀원 분께서 코드를 줄여보는 선택을 하셨더니  
원하는 대로 움직이는 모습이 보였습니다~~!

```pyhton
    elif event.type == KEYDOWN:
        if event.key == K_LEFT:
       #self.board.move_piece(dx=-1, dy=0)
        hold_left = True
    elif event.key == K_RIGHT:
        #self.board.move_piece(dx=1, dy=0)
        hold_right = True
```

이렇게 수정해주고 나면?  
제 깃허브에 올라온 것처럼  
정상적으로 작동이 됩니다!  
여기서 교훈을 하나 얻었습니다...  


***"무언가 더해서 안되면 덜어내라."***  

이번 포스팅은 간단하게 마무리 해보겠습니다!