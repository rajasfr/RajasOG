import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 400, 600
BIRD_WIDTH, BIRD_HEIGHT = 40, 40
PIPE_WIDTH, PIPE_HEIGHT = 60, 400
PIPE_DISTANCE = 200
BIRD_X = 100
GRAVITY = 1
JUMP_STRENGTH = -15
PIPE_SPEED = 5

# Colors
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)

# Create the window
window = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Flappy Bird")

# Bird
bird = pygame.Rect(BIRD_X, HEIGHT // 2, BIRD_WIDTH, BIRD_HEIGHT)
bird_movement = 0

# Pipes
pipes = [pygame.Rect(WIDTH, 0, PIPE_WIDTH, PIPE_HEIGHT)]
pipe_timer = 0

# Game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bird_movement = JUMP_STRENGTH

    # Bird movement
    bird_movement += GRAVITY
    bird.y += bird_movement

    # Pipe movement
    for pipe in pipes:
        pipe.x -= PIPE_SPEED

    # Create new pipes
    pipe_timer += 1
    if pipe_timer == PIPE_DISTANCE:
        new_pipe_height = random.randint(100, 300)
        pipes.append(pygame.Rect(WIDTH, 0, PIPE_WIDTH, new_pipe_height))
        pipes.append(pygame.Rect(WIDTH, new_pipe_height + PIPE_DISTANCE, PIPE_WIDTH, HEIGHT - new_pipe_height - PIPE_DISTANCE))
        pipe_timer = 0

    # Remove off-screen pipes
    pipes = [pipe for pipe in pipes if pipe.x > -PIPE_WIDTH]

    # Check for collisions
    for pipe in pipes:
        if bird.colliderect(pipe):
            pygame.quit()
            sys.exit()

    # Check for bird out of bounds
    if bird.y < 0 or bird.y > HEIGHT:
        pygame.quit()
        sys.exit()

    # Clear the screen
    window.fill(WHITE)

    # Draw pipes
    for pipe in pipes:
        pygame.draw.rect(window, GREEN, pipe)

    # Draw the bird
    pygame.draw.rect(window, GREEN, bird)

    # Update the display
    pygame.display.update()

