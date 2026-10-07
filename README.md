# Tic-Tac-Toe

```python
import random

import pygame

pygame.init()
screen = pygame.display.set_mode((520, 520))
clock = pygame.time.Clock()
border = 10
width=height = 160
done = False
move = 1
win = 0

text = pygame.font.SysFont("Arial", 60)

ls = [[0]*3 for i in range(3)]

def create():
    global ls,win,move
    ls = [[0]*3 for i in range(3)]
    win = 0
    move = 1

def robot():
    variants = []
    corners = [(0,0),(0,2),(2,0),(2,2)]
    for row in range(len(ls)):
        for column in range(len(ls)):
            if ls[row][column] == 0:
                variants.append((row,column))
    if len(variants) == 0:
        return

    for row,col in variants:
        if isWin(row,col,2):
            ls[row][col] = 2
            return
    for row,col in variants:
        if isWin(row,col,1):
            ls[row][col] = 2
            return
    if (1,1) in variants:
        ls[1][1] = 2
        return
    for i in range(len(corners)):
        if corners[i] in variants:
            row,col = corners[i][0],corners[i][1]
            ls[row][col] = 2
            return

    row,col = random.choice(variants)
    ls[row][col] = 2


def checkWin():
    for row in range(len(ls)):
        if ls[row][0] != 0 and ls[row][0]==ls[row][1]==ls[row][2]:
            return ls[row][0]
    for col in range(len(ls)):
        if ls[0][col]!= 0 and ls[0][col]==ls[1][col]==ls[2][col]:
            return ls[0][col]

    if ls[0][0] != 0 and ls[0][0]==ls[1][1]==ls[2][2]:
        return ls[0][0]

    if ls[0][2] != 0 and ls[0][2]==ls[1][1]==ls[2][0]:
        return ls[0][2]

    c = 0
    for row in range(len(ls)):
        for column in range(len(ls)):
            if ls[row][column] == 0:
                c+=1
    if c==0:
        return 3
    return 0

def isWin(row,column,mov):
    if ls[row][column] != 0:
        return False
    ls[row][column]=mov
    res = (checkWin()==mov)
    ls[row][column]=0
    return res

def drawing():
    screen.fill((0,0,0))
    for row in range(len(ls)):
        for col in range(len(ls)):
            x = border+col*(width+border)
            y = border+row*(height+border)
            pygame.draw.rect(screen, (255,255,255), (x,y,width,height))

            move = ls[row][col]
            if move == 1:
                pygame.draw.line(screen, (255,0,0),(x+30,y+30),(x+width-30,y+height-30),5)
                pygame.draw.line(screen,(255,0,0), (x+width-30, y+30),(x+30, y+height-30),5)
            elif move == 2:
                pygame.draw.circle(screen, (0,255,0),(x+width//2,y+height//2),50)
                pygame.draw.circle(screen, (255,255,255),(x+width//2,y+height//2),45)

create()

while not done:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            done = True
        elif event.type == pygame.MOUSEBUTTONDOWN and move == 1 and win == 0:
            mouseX, mouseY = pygame.mouse.get_pos()
            col = (mouseX-border)//(width+border)
            row = (mouseY-border)//(height+border)
            if 0<= row <3 and 0 <= col < 3 and ls[row][col] == 0:
                ls[row][col] = 1
                win = checkWin()
                if win == 0:
                    move = 2
        elif event.type == pygame.KEYDOWN and event.key == pygame.K_r:
            create()

    if win == 0 and move == 2:
        robot()
        win = checkWin()
        if win == 0:
            move = 1

    drawing()

    message = text.render("You win!", True,(0,0,255))
    message2 = text.render("You lose!", True,(0,0,255))
    message3 = text.render("Draw!",True, (0,0,255))

    if win == 1:
        screen.blit(message,(180,220))
    elif win == 2:
        screen.blit(message2,(180,220))
    elif win == 3:
        screen.blit(message3,(180,220))

    pygame.display.update()
pygame.quit()

```
