# Implementation of classic arcade game Pong

import simplegui
import random

# initialize globals - pos and vel encode vertical info for paddles
WIDTH = 600
HEIGHT = 400       
BALL_RADIUS = 20
PAD_WIDTH = 8
PAD_HEIGHT = 80
HALF_PAD_WIDTH = PAD_WIDTH / 2
HALF_PAD_HEIGHT = PAD_HEIGHT / 2
LEFT = False
RIGHT = True
# initialize global variables: paddle positions and velocities. Paddle 1 is left, Paddle 2 is right
paddle1_pos = [[HALF_PAD_WIDTH, HEIGHT / 2 + HALF_PAD_HEIGHT], [HALF_PAD_WIDTH, HEIGHT /2 - HALF_PAD_HEIGHT]]
paddle1_vel = 0
paddle2_pos = [[WIDTH - HALF_PAD_WIDTH, HEIGHT / 2 + HALF_PAD_HEIGHT], [WIDTH - HALF_PAD_WIDTH, HEIGHT / 2 - HALF_PAD_HEIGHT]]
paddle2_vel = 0
acc = 5	     # speed paddles move in pixels per millisecond
score1 = 0
score2 = 0

# initialize ball_pos and ball_vel for new ball in middle of table
# if direction is RIGHT, the ball's velocity is upper right, else upper left
def spawn_ball(direction):
    global ball_pos, ball_vel # these are vectors stored as lists
    ball_pos = [WIDTH / 2, HEIGHT / 2]
    if direction == LEFT:
        ball_vel = [-random.randrange(120, 240) // 100, -random.randrange(60,180) // 100]
    else:
        ball_vel = [random.randrange(120, 240) // 100, -random.randrange(60,180) // 100]

# define event handlers
def new_game():
    global paddle1_pos, paddle2_pos, paddle1_vel, paddle2_vel  # these are numbers
    global score1, score2  # these are ints
    score1 = 0
    score2 = 0
    # spawn ball, random initial choice
    spawn_ball(random.choice([LEFT, RIGHT]))

def draw(canvas):
    global score1, score2, paddle1_pos, paddle2_pos, ball_pos, ball_vel, acc, paddle1_vel, paddle2_vel
    # draw mid line and gutters
    canvas.draw_line([WIDTH / 2, 0],[WIDTH / 2, HEIGHT], 1, "White")
    canvas.draw_line([PAD_WIDTH, 0],[PAD_WIDTH, HEIGHT], 1, "White")
    canvas.draw_line([WIDTH - PAD_WIDTH, 0],[WIDTH - PAD_WIDTH, HEIGHT], 1, "White")        
    # update ball
    ball_pos[0] += ball_vel[0]
    ball_pos[1] += ball_vel[1]
    # collide and reflect off horizontal walls
    if ball_pos[1] <= BALL_RADIUS:
        ball_vel[1] *= -1
    elif ball_pos[1] >= HEIGHT - BALL_RADIUS:
        ball_vel[1] *= -1
    # draw ball
    canvas.draw_circle(ball_pos, BALL_RADIUS, 1, 'white', 'white')
    # update paddle's vertical position, stop movement if it hits the edge
    # this code is gross I'm so sorry
    # paddle2 edge check
    if paddle2_pos[0][1] <= HEIGHT and paddle2_pos >= 0:
        paddle2_pos[0][1] += paddle2_vel    # bottom edge
        paddle2_pos[1][1] += paddle2_vel    # top edge
    if paddle2_pos[0][1] > HEIGHT:
        paddle2_pos[0][1] = HEIGHT
        paddle2_pos[1][1] = HEIGHT - PAD_HEIGHT
        paddle2_vel = 0
    if paddle2_pos[1][1] < 0: # top edge check and fix
        paddle2_pos[1][1] = 0
        paddle2_pos[0][1] = PAD_HEIGHT
        paddle2_vel = 0
    # paddle 1 edge check
    if paddle1_pos[0][1] <= HEIGHT and paddle1_pos >= 0:
        paddle1_pos[0][1] += paddle1_vel    # bottom edge
        paddle1_pos[1][1] += paddle1_vel    # top edge
    if paddle1_pos[0][1] > HEIGHT:
        paddle1_pos[0][1] = HEIGHT
        paddle1_pos[1][1] = HEIGHT - PAD_HEIGHT
        paddle1_vel = 0
    if paddle1_pos[1][1] < 0: # top edge check and fix
        paddle1_pos[1][1] = 0
        paddle1_pos[0][1] = PAD_HEIGHT
        paddle1_vel = 0
    # draw paddles
    canvas.draw_polyline(paddle1_pos, PAD_WIDTH, 'white')
    canvas.draw_polyline(paddle2_pos, PAD_WIDTH, 'white')
    # determine whether paddle and ball collide    
    if ball_pos[0] <= BALL_RADIUS + PAD_WIDTH:
        # left side, check for hit and reflect
        if ball_pos[1] >= paddle1_pos[1][1] and ball_pos[1] <= paddle1_pos[0][1]:
            ball_vel[0] *= -1.1
        else:
        # if not a hit then spawn new ball going right and update score for player 2
            spawn_ball(RIGHT)
            score2 += 1
    elif ball_pos[0] >= WIDTH - BALL_RADIUS - PAD_WIDTH:
        # same but other side
        if ball_pos[1] >= paddle2_pos[1][1] and ball_pos[1] <= paddle2_pos[0][1]:
            ball_vel[0] *= -1.1
        else:
            spawn_ball(LEFT)
            score1 += 1
    # draw scores
    canvas.draw_text(str(score1), (225, 50), 32, 'white')
    canvas.draw_text(str(score2), (350, 50), 32, 'white')
      
def keydown(key):
    global paddle1_vel, paddle2_vel, acc
    # paddle needs to continuously move as button is being pressed
    if key == simplegui.KEY_MAP['down']:
        paddle2_vel += acc
    if key == simplegui.KEY_MAP['up']:
        paddle2_vel -= acc
    if key == simplegui.KEY_MAP['w']:
        paddle1_vel -= acc
    if key == simplegui.KEY_MAP['s']:
        paddle1_vel += acc

def keyup(key):
    global paddle1_vel, paddle2_vel
    if key == simplegui.KEY_MAP['w'] or key == simplegui.KEY_MAP['s']:
        paddle1_vel = 0
    if key == simplegui.KEY_MAP['down'] or key == simplegui.KEY_MAP['up']:
        paddle2_vel = 0
    
# create frame
frame = simplegui.create_frame("Pong", WIDTH, HEIGHT)
frame.set_draw_handler(draw)
frame.set_keydown_handler(keydown)
frame.set_keyup_handler(keyup)
frame.add_button('Reset', new_game, 100)

# start frame
new_game()
frame.start()

