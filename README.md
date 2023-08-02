# --
import pygame

back = (200, 255, 255)
window = pygame.display.set_mode((500, 500))
bg = pygame.transform.scale(pygame.image.load('fon.png'), (500, 500))
window.fill(back)

clock = pygame.time.Clock()
FPS = 60
game = True
finish = False
pygame.font.init()
font1 = pygame.font.SysFont('Ariat', 30)

class GameSprite(pygame.sprite.Sprite):
    def __init__(self, image, x, y, w, h, speed):
        super().__init__()
        self.image = pygame.transform.scale(pygame.image.load(image), (w, h))
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y
        self.speed = speed

    def reset(self):
        window.blit(self.image, self.rect)


class Player(GameSprite):
    def update_l(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[pygame.K_s] and self.rect.y < 550:
            self.rect.y += self.speed

    def update_r(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[pygame.K_DOWN] and self.rect.y < 550:
            self.rect.y += self.speed



rocket1 = Player('racket.png', 10, 200, 50, 150,4)
rocket2 = Player('racket.png', 450, 200, 50, 150,4)
ball = GameSprite('tenis_ball.png', 200, 200, 50, 50, 4)

speed_x = 3
speed_y = 3

while game:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            exit()
    if not finish:
        window.blit(bg, (0, 0))
        rocket1.update_l()
        rocket2.update_r()
        rocket1.reset()
        rocket2.reset()
        ball.reset()
        ball.rect.x += speed_x
        ball.rect.y += speed_y
        if ball.rect.y > 450 or ball.rect.y < 0:
            speed_y *= -1
        
        if pygame.sprite.collide_rect(rocket1, ball) or pygame.sprite.collide_rect(rocket2, ball):
            speed_x *= -1
        if ball.rect.x < 0:
            window.blit(font1.render('PLAYER 1 LOSE', True, (255,0,0)), (250, 250))
            finish = True
        if ball.rect.x > 500:
            window.blit(font1.render('PLAYER 2 LOSE', True, (255,0,0)), (250, 250))
            finish = True
    pygame.display.update()
    clock.tick(FPS)
