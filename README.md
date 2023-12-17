#Создай собственный Шутер!
from pygame import *
from random import randint
display.set_caption("Shooter")
window = display.set_mode((1400, 700))
background = transform.scale(image.load('galaxy.jpg'), (1400, 700))
#font.init()
#font = font.Font(None, 70)
# СДЕЛАТЬ
# кол-во врагов увеличивалось
# создать бонус для стрельбы бешенной
# создать дубликат корабля как бонус
# сердечки интерфейса жизней
# дружеские кораблики по которым нельзя стрелять
# босс лвл сделать
lost = 0 # количество пропущенных врагов######################################3
score = 0 # счетчик убитых
#фоновая музыка
#mixer.init()
#mixer.music.load('space.mp3')
#mixer.music.play()
class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, size_x, size_y, player_speed):
        sprite.Sprite.__init__(self)
        self.image = transform.scale(image.load(player_image), (size_x, size_y))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))
class Player(GameSprite):
    def update(self):
        # самостоятельно
        keys = key.get_pressed()
        if keys[K_a] and self.rect.x >5:
            self.rect.x -= self.speed
        if keys[K_d] and self.rect.x <1360:
            self.rect.x += self.speed
    def fire(self):
        bullet = Bullet('bullet.png', self.rect.centerx, self.rect.top, 15, 20, 10)
        bullets.add(bullet)
class Enemy(GameSprite):
    def update(self):
        self.rect.y += self.speed
        global lost 
        if self.rect.y >= 690:
            lost += 1 
            self.rect.y = randint(-500, -50)
            self.rect.x = randint(100, 1200)
            
class Bullet(GameSprite):
    def update(self):
        self.rect.y -= self.speed
        if self.rect.y < 0:
            self.kill()
class Asteroid(GameSprite):
    def update(self):
        self.rect.y += self.speed
        if self.rect.y >= 800:
            self.rect.y = randint(-500, -50)
            self.rect.x = randint(100, 1200)
ship = Player('rocket.png', 5, 620, 80, 100, 20)
monsters = sprite.Group()
bullets = sprite.Group()
asteroids = sprite.Group()
for i in range(1):
    monster = Enemy("ufo.png", randint(100, 1200), randint(-600, -100), 128, 65, 5)
    monsters.add(monster)
for i in range(1):
    asteroid = Asteroid('asteroid.png', randint(100, 1200), randint(-600, -100), 120, 120, 5)
    asteroids.add(asteroid)
finish = False
game = True
clock = time.Clock()
#fire_sound = mixer.Sound('fire.ogg')
font.init()
font_universal = font.Font(None, 36)
lose = font_universal.render('КАПИБАРЫ КОНЧИЛИСЬ', True, (0, 255, 221))
# также необходимо создать поле текста отвечающее за убитых врагов
# текст который необходимо выводить "Врагов убито:" + переменная score
# отобразить данное поле текста под полем с количеством пропущенных врагов


strelyat_on = False
heart1 = transform.scale(image.load('capybar.png'), (100, 100))# 1
