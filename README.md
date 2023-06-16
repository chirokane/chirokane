- 👋 Hi, I’m @chirokane
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
chirokane/chirokane is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->import pygame
import random

pygame.init()

# Window dimensions
WIDTH = 288
HEIGHT = 512

# Colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)

# Game variables
gravity = 0.25
bird_movement = 0

# Create the game window
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Flappy Bird")

# Load game assets
background_img = pygame.image.load("background.png").convert()
bird_img = pygame.image.load("bird.png").convert()
pipe_img = pygame.image.load("pipe.png").convert()

# Bird position
bird_rect = bird_img.get_rect(center=(50, HEIGHT / 2))

# Pipe variables
pipe_heights = [200, 300, 400]
pipe_list = []
SPAWNPIPE = pygame.USEREVENT
pygame.time.set_timer(SPAWNPIPE, 1200)
pipe_speed = 2

# Game over flag
game_over = False

# Score
score = 0
font = pygame.font.Font("04B_19.ttf", 40)
score_x = 10
score_y = 10

# Game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE and not game_over:
                bird_movement = 0
                bird_movement -= 6
            if event.key == pygame.K_SPACE and game_over:
                game_over = False
                pipe_list.clear()
                bird_rect.center = (50, HEIGHT / 2)
                bird_movement = 0
                score = 0
        if event.type == SPAWNPIPE:
            pipe_list.extend(create_pipe())

    screen.blit(background_img, (0, 0))

    if not game_over:
        # Bird
        bird_movement += gravity
        bird_rect.centery += bird_movement
        screen.blit(bird_img, bird_rect)

        # Pipes
        pipe_list = move_pipes(pipe_list)
        draw_pipes(pipe_list)

        # Collision
        game_over = check_collision(pipe_list)

        # Score
        score += 0.01
        display_score()

    else:
        game_over_screen()

    pygame.display.update()

pygame.quit()


