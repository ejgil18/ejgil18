import pygame
import Projectile

pygame.init()
screen = pygame.display.set_mode((800,600))
pygame.font.init()
myfont = pygame.font.SysFont('Comic Sans MS', 30)
done = False
gameState = "playing"

background = pygame.image.load("Background.png")
player1 = pygame.image.load("smallCharacter.png")
p1x = 100
p1y = 100
p1HitBox = player1.get_rect()
p1Bullets = []
p1Dir = "right"
p1Health = 100

player2 = pygame.image.load("smallCharacter.png")
p2x = 400
p2y = 400
p2HitBox = player2.get_rect()
p2Bullets = []
p2Dir = "right"
p2Health = 100

Rocks = pygame.image.load("rock.png")
Rocks = pygame.transform.scale(Rocks, (30, 30))
r1x = 200
r1y = 200
r2x = 340
r2y = 400
r3x = 480
r3y = 120
r4x = 580
r4y = 300
rockHitBox = Rocks.get_rect()
rockHitBox2 = Rocks.get_rect()
rockHitBox3 = Rocks.get_rect()
rockHitBox4 = Rocks.get_rect()

while not done:
    # required by pygame, check for keypresses ONE time
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            done = True
        if gameState == "playing":
            if event.type == pygame.KEYDOWN and event.key == pygame.K_SPACE:
                p1Bullets.append(Projectile.Bullet(p1x,p1y,p1Dir,5))
            if event.type == pygame.KEYDOWN and event.key == pygame.K_RETURN:
                p2Bullets.append(Projectile.Bullet(p2x,p2y,p2Dir,5))
        if gameState == "game over":
            if event.type == pygame.KEYDOWN and event.key == pygame.K_r:
                p1Bullets = []
                p2Bullets = []
                p1Health = 100
                p2Health = 100
                p1x = 100
                p1y = 100
                p2x = 400
                p2y = 400
                gameState = "playing"


    if gameState == "playing":
        # check for keypresses continually
        pressed = pygame.key.get_pressed()
        if pressed[pygame.K_d]:
            p1x += 3
            p1Dir = "right"
            if p1x > 780:
                p1x = 780
        if pressed[pygame.K_a]:
            p1x -= 3
            p1Dir = "left"
            if p1x < 0:
                p1x = 0

        if pressed[pygame.K_w]:
            p1y -= 3
            p1Dir = "up"
            if p1y < 0:
                p1y = 0
        if pressed[pygame.K_s]:
            p1y += 3
            p1Dir = "down"
            if p1y > 580:
                p1y = 580
        if pressed[pygame.K_RIGHT]:
            p2x += 3
            p2Dir = "right"
            if p2x > 780:
                p2x = 780
        if pressed[pygame.K_LEFT]:
            p2x -= 3
            p2Dir = "left"
            if p2x < 0:
                p2x = 0
        if pressed[pygame.K_UP]:
            p2y -= 3
            p2Dir = "up"
            if p2y < 0:
                p2y = 0
        if pressed[pygame.K_DOWN]:
            p2y += 3
            p2Dir = "down"
            if p2y > 580:
                p2y = 580

        # update hit boxes
        p1HitBox.x = p1x
        p1HitBox.y = p1y
        p2HitBox.x = p2x
        p2HitBox.y = p2y
        rockHitBox.x = r1x
        rockHitBox.y = r1y
        rockHitBox2.x = r2x
        rockHitBox2.y = r2y
        rockHitBox3.x = r3x
        rockHitBox3.y = r3y
        rockHitBox4.x = r4x
        rockHitBox4.y = r4y

        # move the bullets
        for b in p1Bullets:
            b.move()
            if b.isOffScreen() == True:
                p1Bullets.remove(b)
            elif b.isColliding(p2HitBox):
                p1Bullets.remove(b)
                p2Health -= 10
                if p2Health <= 0:
                    gameState = "game over"
                    p2Health = 0
        for b in p2Bullets:
            b.move()
            if b.isOffScreen() == True:
                p2Bullets.remove(b)
            elif b.isColliding(p1HitBox):
                p2Bullets.remove(b)
                p1Health -= 10
                if p1Health <= 0:
                    gameState = "game over"
                    p1Health = 0

        # check for collisions
        if p1HitBox.colliderect(p2HitBox):
            p1HitBox.x = 10
            p1HitBox.y = 10
            p1x = 100
            p1y = 100
        elif p1HitBox.colliderect(rockHitBox):
            if p1x + p1HitBox.width < rockHitBox.x + 5:
                p1x = rockHitBox.x - p1HitBox.width
            if p1x > rockHitBox.x + rockHitBox.width - 5:
                p1x = rockHitBox.x + p1HitBox.width + 10
            if p1y + p1HitBox.height < rockHitBox.y + 5:
                p1y = rockHitBox.y - p1HitBox.height
            if p1y > rockHitBox.y + rockHitBox.height - 5:
                p1y = rockHitBox.y + p1HitBox.height + 10
        elif p2HitBox.colliderect(rockHitBox):
            if p2x + p2HitBox.width < rockHitBox.x + 5:
                p2x = rockHitBox.x - p2HitBox.width
            if p2x > rockHitBox.x + rockHitBox.width - 5:
                p2x = rockHitBox.x + p2HitBox.width + 10
            if p2y + p2HitBox.height < rockHitBox.y + 5:
                p2y = rockHitBox.y - p2HitBox.height
            if p2y > rockHitBox.y + rockHitBox.height - 5:
                p2y = rockHitBox.y + p2HitBox.height + 10
        elif p1HitBox.colliderect(rockHitBox2):
            if p1x + p1HitBox.width < rockHitBox2.x + 5:
                p1x = rockHitBox2.x - p1HitBox.width
            if p1x > rockHitBox2.x + rockHitBox2.width - 5:
                p1x = rockHitBox2.x + p1HitBox.width + 10
            if p1y + p1HitBox.height < rockHitBox2.y + 5:
                p1y = rockHitBox2.y - p1HitBox.height
            if p1y > rockHitBox2.y + rockHitBox2.height - 5:
                p1y = rockHitBox2.y + p1HitBox.height + 10
        elif p2HitBox.colliderect(rockHitBox2):
            if p2x + p2HitBox.width < rockHitBox2.x + 5:
                p2x = rockHitBox2.x - p2HitBox.width
            if p2x > rockHitBox2.x + rockHitBox2.width - 5:
                p2x = rockHitBox2.x + p2HitBox.width + 10
            if p2y + p2HitBox.height < rockHitBox2.y + 5:
                p2y = rockHitBox2.y - p2HitBox.height
            if p2y > rockHitBox2.y + rockHitBox2.height - 5:
                p2y = rockHitBox2.y + p2HitBox.height + 10
        elif p1HitBox.colliderect(rockHitBox3):
            if p1x + p1HitBox.width < rockHitBox3.x + 5:
                p1x = rockHitBox3.x - p1HitBox.width
            if p1x > rockHitBox3.x + rockHitBox3.width - 5:
                p1x = rockHitBox3.x + p1HitBox.width + 10
            if p1y + p1HitBox.height < rockHitBox3.y + 5:
                p1y = rockHitBox3.y - p1HitBox.height
            if p1y > rockHitBox3.y + rockHitBox3.height - 5:
                p1y = rockHitBox3.y + p1HitBox.height + 10
        elif p2HitBox.colliderect(rockHitBox3):
            if p2x + p2HitBox.width < rockHitBox3.x + 5:
                p2x = rockHitBox3.x - p2HitBox.width
            if p2x > rockHitBox3.x + rockHitBox3.width - 5:
                p2x = rockHitBox3.x + p2HitBox.width + 10
            if p2y + p2HitBox.height < rockHitBox3.y + 5:
                p2y = rockHitBox3.y - p2HitBox.height
            if p2y > rockHitBox3.y + rockHitBox3.height - 5:
                p2y = rockHitBox3.y + p2HitBox.height + 10
        elif p1HitBox.colliderect(rockHitBox4):
            if p1x + p1HitBox.width < rockHitBox4.x + 5:
                p1x = rockHitBox4.x - p1HitBox.width
            if p1x > rockHitBox4.x + rockHitBox4.width - 5:
                p1x = rockHitBox4.x + p1HitBox.width + 10
            if p1y + p1HitBox.height < rockHitBox4.y + 5:
                p1y = rockHitBox4.y - p1HitBox.height
            if p1y > rockHitBox4.y + rockHitBox4.height - 5:
                p1y = rockHitBox4.y + p1HitBox.height + 10
        elif p2HitBox.colliderect(rockHitBox4):
            if p2x + p2HitBox.width < rockHitBox4.x + 5:
                p2x = rockHitBox4.x - p2HitBox.width
            if p2x > rockHitBox4.x + rockHitBox4.width - 5:
                p2x = rockHitBox4.x + p2HitBox.width + 10
            if p2y + p2HitBox.height < rockHitBox4.y + 5:
                p2y = rockHitBox4.y - p2HitBox.height
            if p2y > rockHitBox4.y + rockHitBox4.height - 5:
                p2y = rockHitBox4.y + p2HitBox.height + 10

        # draw all the graphics
        screen.blit(background,(0,0))

        #draw the health bars
        pygame.draw.rect(screen,
                         (255,0,0),
                         pygame.Rect(10,10,p1Health,10))
        pygame.draw.rect(screen,
                         (255, 0, 0),
                         pygame.Rect(600, 10, p2Health, 10))

        textsurface = myfont.render("Bullets: " + str(len(p1Bullets)), False, (255, 255, 255))
        screen.blit(textsurface, (0, 0))
        for b in p1Bullets:
            b.draw(screen)
        for b in p2Bullets:
            b.draw(screen)
        pygame.draw.rect(screen,
                         (0,255,0),
                         p1HitBox,
                         1)
        pygame.draw.rect(screen,
                         (0, 255, 0),
                         p2HitBox,
                         1)
        pygame.draw.rect(screen,
                         (0,255,0), rockHitBox,1)
        pygame.draw.rect(screen,
                         (0,255,0),
                         rockHitBox2,
                         1)
        pygame.draw.rect(screen,
                         (0, 255, 0),
                         rockHitBox3,
                         1)
        pygame.draw.rect(screen,
                         (0, 255, 0),
                         rockHitBox4,
                         1)

        screen.blit(player1, (p1x,p1y))
        screen.blit(player2, (p2x, p2y))
        screen.blit(Rocks, (r1x, r1y))
        screen.blit(Rocks, (r2x, r2y))
        screen.blit(Rocks, (r3x, r3y))
        screen.blit(Rocks, (r4x, r4y))
    elif gameState == "game over":
        if p1Health <= 0 and p2Health <= 0:
            textsurface = myfont.render("Tie Game", False, (255, 255, 255))
            screen.blit(textsurface, (400, 300))
        elif p1Health <= 0:
            textsurface = myfont.render("Player 2 Wins", False, (255, 255, 255))
            screen.blit(textsurface, (400, 300))
        elif p2Health <= 0:
            textsurface = myfont.render("Player 1 Wins", False, (255, 255, 255))
            screen.blit(textsurface, (400, 300))



    # Needs to be the LAST LINE
    pygame.display.flip()
