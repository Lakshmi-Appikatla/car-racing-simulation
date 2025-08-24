# car-racing-simulation
A game which is developed by using python code.............
import pygame
import random
import sys

# Initialize pygame
pygame.init()

# Screen size
WIDTH, HEIGHT = 500, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Car Racing Game")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (200, 0, 0)
GREEN = (0, 200, 0)

# Clock
clock = pygame.time.Clock()

# Player Car
car_width, car_height = 50, 100
player_x = WIDTH // 2 - car_width // 2
player_y = HEIGHT - car_height - 10
player_speed = 5

# Enemy Car
enemy_width, enemy_height = 50, 100
enemy_x = random.randint(50, WIDTH - 50 - enemy_width)
enemy_y = -enemy_height
enemy_speed = 5

# Score
score = 0
font = pygame.font.SysFont(None, 40)

def draw_car(x, y, color):
    pygame.draw.rect(screen, color, (x, y, car_width, car_height))

def show_text(text, x, y, color=BLACK):
    label = font.render(text, True, color)
    screen.blit(label, (x, y))

# Game Loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Keys pressed
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < WIDTH - car_width:
        player_x += player_speed

    # Move enemy
    enemy_y += enemy_speed
    if enemy_y > HEIGHT:
        enemy_y = -enemy_height
        enemy_x = random.randint(50, WIDTH - 50 - enemy_width)
        score += 1
        enemy_speed += 0.2  # increase difficulty

    # Collision detection
    if (player_y < enemy_y + enemy_height and
        player_y + car_height > enemy_y and
        player_x < enemy_x + enemy_width and
        player_x + car_width > enemy_x):
        show_text("GAME OVER!", WIDTH//2 - 100, HEIGHT//2, RED)
        pygame.display.update()
        pygame.time.wait(2000)
        pygame.quit()
        sys.exit()

    # Draw background
    screen.fill(WHITE)

    # Draw cars
    draw_car(player_x, player_y, GREEN)
    draw_car(enemy_x, enemy_y, RED)

    # Show Score
    show_text(f"Score: {score}", 10, 10)

    # Update screen
    pygame.display.update()
    clock.tick(60)
