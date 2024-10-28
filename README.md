- 👋 Hi, I’m @Lephikhanh854
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...      GB khanh

<!---
Lephikhanh854/Lephikhanh854 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pygame
import random

# Initialize Pygame
pygame.init()

# Screen dimensions
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# Create the screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption('Mobi Army 3 Demo')

# Player settings
player_size = 50
player_pos = [SCREEN_WIDTH // 2, SCREEN_HEIGHT - player_size - 10]
player_speed = 10

# Bullet settings
bullet_size = 5
bullet_speed = 20
bullets = []

# Enemy settings
enemy_size = 50
enemy_speed = 5
enemies = []
for _ in range(5):
    enemies.append([random.randint(0, SCREEN_WIDTH - enemy_size), random.randint(-600, -50)])

# Clock
clock = pygame.time.Clock()

def detect_collision(player_pos, enemy_pos):
    p_x, p_y = player_pos
    e_x, e_y = enemy_pos
    if (e_x >= p_x and e_x < (p_x + player_size)) or (p_x >= e_x and p_x < (e_x + enemy_size)):
        if (e_y >= p_y and e_y < (p_y + player_size)) or (p_y >= e_y and p_y < (e_y + enemy_size)):
            return True
    return False

game_over = False
score = 0

while not game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_pos[0] > 0:
        player_pos[0] -= player_speed
    if keys[pygame.K_RIGHT] and player_pos[0] < SCREEN_WIDTH - player_size:
        player_pos[0] += player_speed
    if keys[pygame.K_SPACE]:
        bullets.append([player_pos[0] + player_size//2, player_pos[1]])

    screen.fill(BLACK)

    for bullet in bullets:
        bullet[1] -= bullet_speed
        pygame.draw.rect(screen, RED, (bullet[0], bullet[1], bullet_size, bullet_size))
        if bullet[1] < 0:
            bullets.remove(bullet)

    for enemy_pos in enemies:
        if detect_collision(player_pos, enemy_pos):
            game_over = True
        enemy_pos[1] += enemy_speed
        if enemy_pos[1] > SCREEN_HEIGHT:
            enemy_pos[0] = random.randint(0, SCREEN_WIDTH - enemy_size)
            enemy_pos[1] = random.randint(-600, -50)
            score += 1
        pygame.draw.rect(screen, GREEN, (enemy_pos[0], enemy_pos[1], enemy_size, enemy_size))

    pygame.draw.rect(screen, WHITE, (player_pos[0], player_pos[1], player_size, player_size))

    pygame.display.update()
    clock.tick(30)

pygame.quit()
print("Your Score: ", score)
