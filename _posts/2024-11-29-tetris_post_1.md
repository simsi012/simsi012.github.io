---
layout: post
title: 우당탕탕 테트리스 게임 개발기_[1]
author: simsi012
date: 2024-11-29 17:12:00 +0800
categories: [Work, Making]
tags: [Make, Work]
published: true
---

*** 안녕하십니까 여러분 ***  
늘 삶이 일정하게 우당탕탕 흘러가는 주인장입니다.  
오늘은 테트리스 게임 개발기를 한번 리뷰해보려 합니다.  
3부작 정도 될꺼 같은데, 주로 코드를 어떻게 고치고 해결했나?  
초점을 두고 리뷰해보려고 합니다.  

## 일단 호기롭게 시작을 해보자!  
  
먼저 괜찮게 작동하는 코드를 찾아봅시다.  
음 어떤게 좋을지 고민하던차...  
![original_tetris_source_github](https://github.com/simsi012/simsi012.github.io/blob/main/assets/img/tetris_origianl_github.png?raw=true)  
오 꽤 해볼만한 코드를 발견했습니다!  
<div class="tenor-gif-embed" data-postid="15449497793648793961" data-share-method="host" data-aspect-ratio="0.971888" data-width="100%"><a href="https://tenor.com/view/sigma-gif-15449497793648793961">Sigma GIF</a>from <a href="https://tenor.com/search/sigma-gifs">Sigma GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>
  
  
이정도면 시작부터 꽤 희극적으로 돌아가는거 같군요!  
그렇다면 이제 한번 실행을 해보고 돌려봐야하는데...  
(자고로 주인장은 게임 안함)  
<div class="tenor-gif-embed" data-postid="7182617338731658942" data-share-method="host" data-aspect-ratio="1.63816" data-width="100%"><a href="https://tenor.com/view/oh-sorry-sad-sad-gif-i%27m-sorry-gif-7182617338731658942">Oh Sorry GIF</a>from <a href="https://tenor.com/search/oh-gifs">Oh GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>  
해보니 나름의 문제가 있었습니다...  
  
그거슨 블록이 내려가지 않은 상황이었는데,  
1. 좌우 방향키를 눌렀을 때
2. 상하 방향키를 눌렀을 때  
이 두가지 케이스가 첫번째 난관이었더군요 호호...  
  
어쩌겠어요, 해결 해야겠죠?  

원본 코드는 이렇습니다  
```python
 self.screen = pygame.display.set_mode((P_UI.WIDTH, P_UI.HEIGHT))
        self.clock = pygame.time.Clock()
        self.board = Board(self.screen)
        pygame.init()
        pygame.display.set_caption('Tetris_by_Lim')
        pygame.time.set_timer(Tetris.DROP_EVENT, 300)
        self.main_sound('back.wav')
        self.start()       


        self.board.draw_static_block(4, 80, hei+80, size)
        self.board.draw_static_block(1, 200, hei+80, size)
        self.board.draw_static_block(6, 330, hei+80, size)
        image = pygame.image.load('./materials/image/logo.png')
        self.screen.blit(image, (80, 50))
        image2 = pygame.image.load('./materials/image/start.jpg')
        self.screen.blit(image2, (140, 380))


    def handle_key(self, event_key):
        if event_key == K_DOWN:
            self.board.drop_piece()
        elif event_key == K_LEFT:
            self.board.move_piece(dx=-1, dy=0)
        elif event_key == K_RIGHT:
            self.board.full_drop_piece()
        elif event_key == K_ESCAPE:
            self.pause()
            # self.pause_sound.play()
        elif event_key == K_LSHIFT  :
            self.board.hold_block()
            # self.hold_sound.play()
        # elif event_key == K_q
            # self.board.rotate_piece(False)
        else : pass

    def push_key(self):
        press = pygame.key.get_pressed()
        if press[pygame.K_DOWN] : 
            self.board.set_timer(80)
        else :
            self.board.set_timer(P_UI.SPEED)
```  
음 자고로 주인장은 포스팅한 년도 6월에 갓전역한 따끈따끈 새삥 복학생이기에,  
군대에서 한거라곤 전공 책도 아닌 수학책만 들여다봤기에 코드를 모르겠더군요 허허..  
일단 코드에 대한 이해는 우리 올트먼 형님이 대표로 계신 GPT에게 물어봤습니다.  
물어보니 위의 코드 부분이 직접적인 조작 부분이니 수정하면 될꺼 같다고 합니다.  
  
<div class="tenor-gif-embed" data-postid="8065719971509630412" data-share-method="host" data-aspect-ratio="1.94531" data-width="100%"><a href="https://tenor.com/view/hmm-interesting-intrigued-curious-thinking-gif-8065719971509630412">Hmm Interesting GIF</a>from <a href="https://tenor.com/search/hmm-gifs">Hmm GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>  
흠 ... 문제는 뭔지 알겠는데...  
파이썬 코드를 어케 짜야하지?  
  
<div class="tenor-gif-embed" data-postid="13701027903669677755" data-share-method="host" data-aspect-ratio="1.29016" data-width="100%"><a href="https://tenor.com/view/greg-davies-never-mind-the-buzzcocks-shouting-yelling-yell-gif-13701027903669677755">Greg Davies Never Mind The Buzzcocks GIF</a>from <a href="https://tenor.com/search/greg+davies-gifs">Greg Davies GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>
목놓아 GPT를 다시 불렀습니다..  

일단 기존 코드에 코드를 추가해줬어야 했씁니다.  
단락 단위로 설명해볼게요.  

```python
        self.screen = pygame.display.set_mode((P_UI.WIDTH, P_UI.HEIGHT))
        self.clock = pygame.time.Clock()
        self.board = Board(self.screen)
        self.is_fast_drop = False
        pygame.init()
        pygame.display.set_caption('Tetris_by_Lim')
        pygame.time.set_timer(Tetris.DROP_EVENT, 300)
        pygame.time.set_timer(Tetris.DROP_EVENT, P_UI.SPEED)
        self.main_sound('back.wav')
        self.start()  
```  
위에 코드는 수정된 코드 첫 단락입니다.  
  
```python
        self.screen = pygame.display.set_mode((P_UI.WIDTH, P_UI.HEIGHT))
        self.clock = pygame.time.Clock()
        self.board = Board(self.screen)
```
이 부분 밑에 바로 코드를 하나 추가해줬습니다.  
  
```python
        self.is_fast_drop = False
```  
  
게임 블록이 적절하게 떨어지기 위해 코드를 삽입한 부분입니다.  
  
False로 설정하여 개발자가 속도를 조절할 수 있도록 기초작업을 했습니다.  
  
다음으로는  
```python
        pygame.init()
        pygame.display.set_caption('Tetris_by_Lim')
        pygame.time.set_timer(Tetris.DROP_EVENT, 300)
```  
위 코드에  
  
```python  
        pygame.time.set_timer(Tetris.DROP_EVENT, P_UI.SPEED)
```  
  
맨 밑부분에 이 코드도 삽입해줍니다.  
  
게임 속도를 정의하는 값으로, 초기 DROP_EVENT와 업데이트 속도를 조절합니다.  
  
자 여기까지가 원하는 동작의 기반 작업이라고 할 수 있습니다.  
  
다음 부분은  
```python
    def handle_key(self, event_key):
        if event_key == K_DOWN:
            self.board.drop_piece()
        elif event_key == K_LEFT:
            self.board.move_piece(dx=-1, dy=0)
        elif event_key == K_RIGHT:
            self.board.full_drop_piece()
        elif event_key == K_ESCAPE:
            self.pause()
            # self.pause_sound.play()
        elif event_key == K_LSHIFT  :
            self.board.hold_block()
            # self.hold_sound.play()
        # elif event_key == K_q
            # self.board.rotate_piece(False)
        else : pass
```  
요 부분인데,  

```python
    def handle_key(self, event_key):
        if event_key == K_DOWN:
            self.board.drop_piece()  # 타이머를 빠르게 설정하여 블록이 빠르게 내려가도록 함
        elif event_key == K_LEFT:
            self.board.move_piece(dx=-1, dy=0)
        elif event_key == K_RIGHT:
            self.board.full_drop_piece()
        elif event_key == K_ESCAPE:
            self.pause()
        elif event_key == K_LSHIFT:
            self.board.hold_block()
        else:
            pass
```  
이렇게 불필요한 주석도 제거해줍니다.  
다음이 진짜 대규모 패치가 일어났습니다..  
  
```python
    def push_key(self):
        press = pygame.key.get_pressed()
        if press[pygame.K_DOWN] : 
            self.board.set_timer(80)
        else :
            self.board.set_timer(P_UI.SPEED)
```  
이런 코드였는데  
  
```python
    def push_key(self):
        press = pygame.key.get_pressed()
    
        # 대각선 이동: K_DOWN과 K_LEFT 또는 K_RIGHT가 동시에 눌렸을 때
        if press[pygame.K_DOWN] and press[pygame.K_LEFT]:
            self.board.move_piece(dx=-1, dy=1)  # 왼쪽 대각선 아래로 한 칸 이동
        elif press[pygame.K_DOWN] and press[pygame.K_RIGHT]:
            self.board.move_piece(dx=1, dy=1)  # 오른쪽 대각선 아래로 한 칸 이동
        else:
            # 일반 하강: K_DOWN 키만 눌렸을 때 한 칸 아래로 이동
            if press[pygame.K_DOWN]: 
                self.board.drop_piece()
            # 일반 좌우 이동: K_LEFT 또는 K_RIGHT 키가 눌렸을 때 한 칸씩 이동
            if press[pygame.K_LEFT]:
                self.board.move_piece(dx=-1, dy=0)
            elif press[pygame.K_RIGHT]:
                self.board.move_piece(dx=1, dy=0)
```  
이렇게 코드를 더 넣어줬어야 했습니다.  

코드를 보면 동시에 키가 눌렸을 때 이동을 할 수 있도록 따로 함수를 넣었습니다.  

마침 잘 작동되는 것을 보고..  
  
<div class="tenor-gif-embed" data-postid="3522541636624242111" data-share-method="host" data-aspect-ratio="0.564257" data-width="100%"><a href="https://tenor.com/view/mike-myers-mikemyers-austin-powers-austin-powers-gif-yeah-baby-gif-3522541636624242111">Mike Myers Mikemyers GIF</a>from <a href="https://tenor.com/search/mike+myers-gifs">Mike Myers GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>  
  
감출 수 없는 기쁨...  

근데 또 다른 문제가 기다리고 있었습니다...  
그것은 다음 포스팅에...  
<div class="tenor-gif-embed" data-postid="24882272" data-share-method="host" data-aspect-ratio="1.76796" data-width="100%"><a href="https://tenor.com/view/to-be-continued-one-piece-gif-24882272">To Be Continued One Piece GIF</a>from <a href="https://tenor.com/search/to+be+continued+one+piece-gifs">To Be Continued One Piece GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>