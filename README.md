import pygame
import sys
import time

# Initialize Pygame
pygame.init()

# Set up screen
screen = pygame.display.set_mode((640, 480))

# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLACK = (0, 0, 0)

# Font for Game Over text
font = pygame.font.SysFont(None, 55)

# Font for win game text
font = pygame.font.SysFont(None, 55)

# Positions
player_x = 320
enemy_x, enemy_y = 0, 0

# Enemy speed
enemy_speed_x, enemy_speed_y = 0.15, 0.15

# Start time
start_time = time.time()

def display_game_over():
    game_over_text = font.render("Game Over", True, 'WHITE')
    screen.blit(game_over_text, (220, 200))
    pygame.display.flip()

def display_you_win():
    you_win_text = font.render("You Win", True, 'white')
    screen.blit(you_win_text, (220, 200))
    pygame.display.flip()



# Game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Move player
    keys = pygame.key.get_pressed()
    player_x += (keys[pygame.K_RIGHT] - keys[pygame.K_LEFT]) * 0.5

    # Ensure player stays within screen bounds
    player_x = max(0, min(player_x, 620))

    # Move enemy
    enemy_x += enemy_speed_x
    enemy_y += enemy_speed_y

    # Boundary checking for enemy
    if enemy_x > 620 or enemy_x < 0:
        enemy_speed_x *= -1
    if enemy_y > 460 or enemy_y < 0:
        enemy_speed_y *= -1

    # Ensure enemy stays within screen bounds
    enemy_x = max(0, min(enemy_x, 620))
    enemy_y = max(0, min(enemy_y, 460))

    # Check for collision
    player_rect = pygame.Rect(player_x, 450, 20, 20)
    enemy_rect = pygame.Rect(enemy_x, enemy_y, 20, 20)
    if player_rect.colliderect(enemy_rect):
        display_game_over()
        time.sleep(2)
        pygame.quit()
        sys.exit()


    if time.time() - start_time > 20:
        display_you_win()
        time.sleep(2)
        pygame.quit()
        sys.exit()

    # Draw everything
    screen.fill(BLACK)
    pygame.draw.rect(screen, WHITE, player_rect)
    pygame.draw.rect(screen, RED, enemy_rect)

    # Update display
    pygame.display.flip()
