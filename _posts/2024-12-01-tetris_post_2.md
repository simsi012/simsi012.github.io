---
layout: post
title: 우당탕탕 테트리스 게임 개발기_[3]
author: simsi012
date: 2024-12-01 20:45:00 +0800
categories: [Work, Making]
tags: [Make, Work]
published: true
---

***안녕하십니까 여러분***  

이번 포스팅도 테트리스 게임 개발기입니다.  

저번에는 테트리스 기존 코드들의 버그를 수정했었는데,  
요번에는 테트리스 게임 기능 구현으로 넘어갔습니다!
  
이제 기존 테트리스 게임이 원하는 대로 잘 작동하더군요 호호 
   
<div class="tenor-gif-embed" data-postid="4811540142605673641" data-share-method="host" data-aspect-ratio="1" data-width="100%"><a href="https://tenor.com/view/wow-amused-really-clapping-clapping-hands-gif-4811540142605673641">Wow Amused GIF</a>from <a href="https://tenor.com/search/wow-gifs">Wow GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>  
  
그러니 이제 슬슬 기능을 몇개 추가해봐야겠어요 ㅎㅎ  

팀원들과 추가하려는 기능은 총 3개입니당.

1. 블록의 높이를 UI로 나타내기!
2. 높이가 60프로 이상 시 배경 전환!
3. 60프로 이상 시 음원이 바뀐다!  
  
이렇게 3개인데,,,  
  
직감상으로는 기존 코드에 기능 몇개?  
정도만 추가하면 되는 것이니 그렇게 어렵진 않겠지!!  
싶었으나...  

<div class="tenor-gif-embed" data-postid="24307771" data-share-method="host" data-aspect-ratio="1" data-width="100%"><a href="https://tenor.com/view/this-is-too-hard-bro-cratos-this-is-pretty-tough-its-really-difficult-xset-gif-24307771">This Is Too Hard Bro Cratos GIF</a>from <a href="https://tenor.com/search/this+is+too+hard+bro-gifs">This Is Too Hard Bro GIFs</a></div> <script type="text/javascript" async src="https://tenor.com/embed.js"></script>  
  
역시 인생은 쉽지 않습니다.  

당연히 수월하겠거니 했는데 ㅠㅠ  
  
생각대로 코드가 안돌아가서 열심히 gpt에게 묻고 다시 생각해서 코드를 짜내보고 그랬습니다.  
  
이번 포스팅에서는 1번째 현재 높이를 나타내주는 UI를 구현하는 과정을 리뷰해보겠습니다!  
  
## 1. 현재 높이를 Percentage로 구현!

1단계는 어렵지 않았습니다!  
  
```python
    def draw_board(self):
        x_pix, y_pix = self.tetris_location(0, 0)
        x_end = x_pix + self.t_width * self.block_size
        y_end = y_pix + self.t_height * self.block_size
        self.screen.fill(P_UI.backcolor)       
        pygame.draw.circle(self.screen, P_UI.grey_2, (x_pix, y_end), 6, 0)
        pygame.draw.circle(self.screen, P_UI.grey_2, (x_end, y_end), 6, 0)

        pygame.draw.rect(self.screen,
                    P_UI.grey_2,
                    Rect(x_pix, y_pix + self.block_size, 
                    self.t_width * self.block_size, (self.t_height-1) * self.block_size),13)        
        pygame.draw.rect(self.screen,
                    P_UI.t_background,
                    Rect(x_pix, y_pix + self.block_size, 
                    self.t_width * self.block_size, (self.t_height-1) * self.block_size))


        font0 = pygame.font.Font(P_UI.path, 25)
```  
위 코드가 기존 코드였는데  
여기에 화면에 높이가 나타나도록 코드 몇개를 추가해줬습니다!
  
```python
    def draw_board(self):
        x_pix, y_pix = self.tetris_location(0, 0)
        x_end = x_pix + self.t_width * self.block_size
        y_end = y_pix + self.t_height * self.block_size
        self.screen.fill(P_UI.backcolor)       
        pygame.draw.circle(self.screen, P_UI.grey_2, (x_pix, y_end), 6, 0)
        pygame.draw.circle(self.screen, P_UI.grey_2, (x_end, y_end), 6, 0)

        pygame.draw.rect(self.screen,
                    P_UI.grey_2,
                    Rect(x_pix, y_pix + self.block_size, 
                    self.t_width * self.block_size, (self.t_height-1) * self.block_size),13)        
        pygame.draw.rect(self.screen,
                    P_UI.t_background,
                    Rect(x_pix, y_pix + self.block_size, 
                    self.t_width * self.block_size, (self.t_height-1) * self.block_size))
        
        # 높이 계산 및 퍼센트 변환
        highest = self.calculate_highest()
        height_percentage = int((highest / self.t_height) * 100)  # 퍼센트 단위로 계산

        # 최고 높이 표시 추가
        font_high = pygame.font.Font(P_UI.path, 25)
        text_high = font_high.render("HEIGHT", 1, P_UI.black)
        num_high = pygame.font.Font(P_UI.path, 30).render(f"{height_percentage}%", 1, P_UI.black)
        self.screen.blit(text_high, (392, 390))  # HEIGHT 텍스트 위치
        self.screen.blit(num_high, (410, 420))  # 퍼센트 출력 위치

        font0 = pygame.font.Font(P_UI.path, 25)
        self.draw_board()
        self.draw_blocks(self.piece, dx=self.piece_x, dy=self.piece_y)
        self.draw_blocks(self.board, board=1)
    
    #추가한 코드 Start
    def calculate_highest(self):
        for y in range(self.t_height):
            if any(self.board[y]):
                return self.t_height - y
        return 0
         # 블록이 없을 경우 최고 높이 0
    #추가한 코드 End
```  
  
완성된 코드는 위와 같습니다!  

여기서 추가된 코드는 !

```python
    #추가한 코드 Start
    def calculate_highest(self):
        for y in range(self.t_height):
            if any(self.board[y]):
                return self.t_height - y
        return 0
    # 블록이 없을 경우 최고 높이 0
    #추가한 코드 End
```  
이 부분과  
  


``` python
        # 높이 계산 및 퍼센트 변환
        highest = self.calculate_highest()
        height_percentage = int((highest / self.t_height) * 100)  # 퍼센트 단위로 계산
        # 최고 높이 표시 추가
        font_high = pygame.font.Font(P_UI.path, 25)
        text_high = font_high.render("HEIGHT", 1, P_UI.black)
        num_high = pygame.font.Font(P_UI.path, 30).render(f"{height_percentage}%", 1, P_UI.black)
        self.screen.blit(text_high, (392, 390))  # HEIGHT 텍스트 위치
        self.screen.blit(num_high, (410, 420))  # 퍼센트 출력 위치
```
  
이 부분입니다! 
  
하나하나 차례대로 설명해볼게요!

# 1-1. 일단 높이를 계산해주는 코드를 짜볼까요?

우선 ! 높이를 알기 위해 최고 높이를 반환해주는 함수 코드가 필요합니다!
  
```python
    #추가한 코드 Start
    def calculate_highest(self):
        for y in range(self.t_height):
            if any(self.board[y]):
                return self.t_height - y
        return 0
    # 블록이 없을 경우 최고 높이 0
    #추가한 코드 End
        
```
- 최고 높이를 알려주는 함수를 만듭니다.
-  구성요소에는 총 높이를 나타내는 배열 -
   ```python 
   self.t_height 
   ```  
     
   2차원 배열의 행을 통해 특정 y 레벨의 블록 존재 여부를 확인하는
   ```python 
   self.board[y]  
   ```  
   해당 행에 하나 이상의 블록이 있는지 확인하는 - if any(self.board[y])  
   ```python
   if any(self.board[y])
   ```
   가장 위에 블록이 존재하는 행으로부터 남은 높이를 계산 후 반환한 뒤,
   보드의 총 높이에서 블록이 있는 첫 번째 행을 빼는  
   ```python
   return self.t_height - y
   ```  

   보드에 블록이 전혀 없다면 
   ```python
   return 0  
   ```  
- 위와 같은 구성요소로 코드를 짭니다!  

쉽게 설명하자면,  
1. 가로 줄에 블록이 하나라도 있다?  
2. 세로 줄에 블록이 존재 한다고 간주한다
3. 그럼 위에서 아래로 탐색하며
4. 블록이 있는 첫번째 행을 탐색합니다.
5. 그럼 그 길이만큼을 전체 길이에서 뺀 값을 최고 높이로 대입합니다.
  
자 이제 최고 높이를 알았으니 화면에 띄워야겠죠?  
  
``` python
        # 높이 계산 및 퍼센트 변환
        highest = self.calculate_highest()
        height_percentage = int((highest / self.t_height) * 100)  # 퍼센트 단위로 계산
        # 최고 높이 표시 추가
        font_high = pygame.font.Font(P_UI.path, 25)
        text_high = font_high.render("HEIGHT", 1, P_UI.black)
        num_high = pygame.font.Font(P_UI.path, 30).render(f"{height_percentage}%", 1, P_UI.black)
        self.screen.blit(text_high, (392, 390))  # HEIGHT 텍스트 위치
        self.screen.blit(num_high, (410, 420))  # 퍼센트 출력 위치
```  
  
이 코드로 화면에 보이는 동작을 하게 됩니다!  
이 코드 또한 설명을 해보자면,  

- 현재 최고 높이를 기존에 적어둔 함수로 가져옵니다 
  ```python
  highest = self.calculate_highest()
  ```
- 퍼센트 단위로 계산해줍니다
  ```python
  height_percentage = int((highest / self.t_height) * 100)
  ```
- 그리고 계산한 값을 UI로 구현해줍니다.
  ```python
        font_high = pygame.font.Font(P_UI.path, 25)
        text_high = font_high.render("HEIGHT", 1, P_UI.black)
        num_high = pygame.font.Font(P_UI.path, 30).render(f"{height_percentage}%", 1, P_UI.black)
        self.screen.blit(text_high, (392, 390))  # HEIGHT 텍스트 위치
        self.screen.blit(num_high, (410, 420))  # 퍼센트 출력 위치
  ```
  
이렇게 구현해주면? 

![파이썬 퍼센티지 출력]()
  
위의 사진처럼 나오게 됩니다!  

이번 포스팅에서는 최고 높이 비율을 화면 상에 나타내어보는 기능을 구현해봤습니다.  
다음에는 60프로 이상 블록이 쌓일 시 배경이 깜빡거리는 기능을 구현해보겠습니다!  