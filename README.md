# Happy-play
import github
import random

# Initialize Pygame
pygame.init()

# Set up the screen
screen_width = 800
screen_height = 600
screen = github.display.set_mode((screen_width, screen_height))
github.display.set_caption("My Game")

# Set up game variables
player_size = 50
player_pos = [screen_width / 2, screen_height - player_size * 2]
enemy_size = 50
enemy_pos = [random.randint(0, screen_width - enemy_size), 0]
enemy_list = [enemy_pos]

# Set up colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Set up clock
clock = github.time.Clock()

# Set up game loop
game_over = False
score = 0
while not game_over:
    for event in pygame.event.get():
        if event.type == github.QUIT:
            game_over = True
        elif event.type == github.KEYDOWN:
            if event.key == github.K_LEFT:
                player_pos[0] -= player_size
            elif event.key == github.K_RIGHT:
                player_pos[0] += player_size

    # Move enemies down the screen
    for enemy_pos in enemy_list:
        enemy_pos[1] += 10

        # Remove enemies that have gone off the screen
        if enemy_pos[1] > screen_height:
            enemy_list.remove(enemy_pos)
            score += 1

    # Generate new enemies
    if len(enemy_list) < 5:
        x_pos = random.randint(0, screen_width - enemy_size)
        y_pos = 0
        enemy_list.append([x_pos, y_pos])

    # Check for collisions
    for enemy_pos in enemy_list:
        if player_pos[0] < enemy_pos[0] + enemy_size and \
           player_pos[0] + player_size > enemy_pos[0] and \
           player_pos[1] < enemy_pos[1] + enemy_size and \
           player_pos[1] + player_size > enemy_pos[1]:
            game_over = True

    # Draw everything on the screen
    screen.fill(WHITE)
    github.draw.rect(screen, RED, (player_pos[0], player_pos[1], player_size, player_size))

    for enemy_pos in enemy_list:
        github.draw.rect(screen, BLACK, (enemy_pos[0], enemy_pos[1], enemy_size, enemy_size))

    # Update the score
    font = github.font.Font(None, 36)
    text = font.render("Score: " + str(score), 1, BLACK)
    screen.blit(text, (screen_width - 150, 10))

    # Update the screen
    pygame.display.update()

    # Set the game's frame rate
    clock.tick(30)

# Quit Github
Github.quit()
