# Ping-Pong
import pygame

pygame.display.set_caption("Evan's Cool Game!")
pygame.image.load("download.jpeg")

class GameSprite(pygame.sprite.Sprite):
    def __init__(self, filename, x, y, width, height, speed):
        super().__init__()
        print(filename)
        self.image = pygame.transform.scale(pygame.image.load(filename), (width, height))
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.speed = speed
    def reset(self):
        screen.blit(self.image, (self.rect.x, self.rect.y))      
class Ball(GameSprite):
    def __init__(self, filename, x, y, speed):
        super().__init__(filename, x, y, 100, 100, speed)
        self.target_y = 800
    #def update(self):
class Player(GameSprite):
    def __init__(self, x, y, speed, image, left=pygame.K_a, right=pygame.K_d):
        super().__init__(image, x, y, 100, 100, speed)        
        self.left = left
        self.right = right
    def update(self):
        keys = pygame.key.get_pressed()
        if keys[self.left]:
            self.rect.x -= self.speed
        if keys[self.right]:
            self.rect.x += self.speed


pygame.init()
screen = pygame.display.set_mode((1280, 720))
clock = pygame.time.Clock()
running = True
dt = 0
background_image = pygame.image.load("download.jpeg")
background_image = pygame.transform.scale(background_image, (1280, 720))

player = Player(100,100, 5, "enemy.png", left=pygame.K_a, right=pygame.K_d)
player2 = Player (40,40, 10, "Circle.jpeg", left=pygame.K_LEFT, right=pygame.K_RIGHT)
ball = Ball("Circle.jpeg", 40,40, 10)
screen.blit(background_image, (0,0)) 
running = True
score = 0
gameover = False
#point_text = my_font.render("score:" +str(score), True, "Red")
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
    screen.blit(background_image, (0,0)) 
    #screen.blit(point_text, (50,50))
    keys_pressed = pygame.key.get_pressed()
    if not gameover:
        player.update()
        player.reset()
        player2.update()
        player2.reset()
        ball.update()
        ball.reset()
        if ball.rect.colliderect(player.rect):
            ball.rect.y = ball.rect.y * -1
        if ball.rect.colliderect(player2.rect):
            ball.rect.y = ball.rect.y * -1
        if score == 10:
            gameover = True
            pygame.mixer.music.stop()
            screen.fill("black")
    else:
        screen.fill("black")
        screen.blit(end_text, (200,200))
    pygame.display.flip()
    dt = clock.tick(60) / 1000
    clock.tick(60)
