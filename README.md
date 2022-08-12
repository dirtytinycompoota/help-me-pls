# help-me-pls
#help me find the error pls (this is pygame)

import pygame
import random
import math

pygame.init()
	
screen = pygame.display.set_mode((640, 600))
	
bg = pygame.image.load('bg.jpg')
	
pygame.display.set_caption("space game")
icon = pygame.image.load('ufo.png')
pygame.display.set_icon(icon)


playerimg = pygame.image.load('player.png')
playerx = 295
playery = 480
playerx_change = 0
	
alienimg = []
alienx = []
alieny = []
alienx_change = []
alieny_change = []
num_alien = 6

for i in range(num_alien):
	alienimg.append(pygame.image.load('alien.png'))
	alienx.append(random.randint(0,576))
	alieny.append(random.randint(50, 150))
	alienx_change.append(4)
	alieny_change.append(40)

bulletimg = pygame.image.load('bullet.png')
bulletx = 0
bullety = 480
bulletx_change = 0
bullety_change = 3
bullet_state = "ready"

score_value = 0
font = pygame.font.Font('freesansbold.ttf', 32)

textx = 10
texty = 10

gg_font = pygame.font.Font('freesansbold.ttf', 64)

def show_score(x, y):
	score = font.render('score : ' + str(score_value), True, (255,255,255))
	screen.blit(score, (x, y))

def gg_text():
	gg_text = font.render('GAMEOVER', True, (255, 255, 255))
	screen.blit(gg_text, (200, 250))

def player(x,y):
	screen.blit(playerimg, (x, y))

def alien(x,y,i):
	screen.blit(alienimg[i], (x, y))

def bullet(x,y):
	global bullet_state
	bullet_state = "fire"
	screen.blit(bulletimg, (x + 16, y + 10))

def collision(alienx, alieny, bulletx, bullety):
	distance = math.sqrt((math.pow(alienx - bulletx, 2)) + (math.pow(alieny - bullety, 2)))
	if distance < 27:
		return True
	else:
		return False

running = True
while running:

	screen.blit(bg, (0,0))
	for event in pygame.event.get():
		if event.type == pygame.QUIT:
			running = False

		if event.type == pygame.KEYDOWN:
			if event.key == pygame.K_LEFT:
				playerx_change = -5
			if event.key == pygame.K_RIGHT:
				playerx_change = 5
			if event.key == pygame.K_SPACE:
				if bullet_state == "ready":
					bulletx = playerx
					bullet(bulletx, bullety)
		if event.type == pygame.KEYUP:
			if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
				playerx_change = 0

	playerx += playerx_change
	if playerx <= 0:
		playerx = 0
	elif playerx >= 576:
		playerx = 576
		
		alienx[i] += alienx_change[i]
		if alienx[i] <= 0:
			alienx_change[i] = 3
			alieny[i] += alieny_change[i]
		elif alienx[i] >= 736:
			alienx_change[i] = -3
			alieny[i] += alieny_change[i]

		for i in range(num_alien):
			if alieny[i] > 200:
				for j in range(num_alien):
					alieny[j] = 2000
					gg_text()
					break
		
		collision = collision(alienx[i], alieny[i], bulletx, bullety)
		if collision:
			bullety = 480
			bullet_state = "ready"
			score_value += 1
			alienx[i] = (random.randint(0, 576))
			alieny[i] = (random.randint(50, 150))
			print(score_value)

		if bullety <= 0:
			bullety = 480
			bullet_state = "ready"

		if bullet_state == "fire":
			bullet(bulletx, bullety)
			bullety -= bullety_change
		
		player(playerx, playery)
		alien(alienx[i], alieny[i], i)
		pygame.display.update()
