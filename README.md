# Breakout-Game
Report of Breakout game coded in MIPS assembly language. Live demo upon request.


# Video of Breakout Game
https://user-images.githubusercontent.com/98304680/211619985-60a58c71-df0c-4fe2-a1f4-8b9cc29e0990.mov


# Static Image of the Design

<img width="258" alt="breakout-image" src="https://user-images.githubusercontent.com/98304680/211613645-3b62b0d1-c02f-4ab4-93d1-851d5cb97505.png">

# How To Play

    1. Open breakout.asm using Mars 4.5
    
    2. Under Tools, open Bitmap Display and configure unit width and unit height in pixels to be 4. 
       Set the display height and width in pixels to be 256 and the base address for the display to 0x10008000 ($gp).
    
    3. Under Tools, open Keyboard and Display MMIO Simulator.
    
    4. Connect both of the Bitmap Display and Keyboard and Display MMIO Simulator to MIPS
    
    5. Under Run, click Assemble
    
    6. Under Run, click Go
    
    7. Click the Keyboard and Display MMIO Simulator window to enable the control
    
    8. You can use a, d on the keyboard to move the bottom paddle. 

    9. You can use j, l on the keyboard to move the top paddle. 

    10. You can use ESC on the keyboard to quit the game and p on the keyboard to pause/unpause 
        the game.
    
    11. Try to destroy all the bricks without dropping the ball to the ground! 
        You have three lives before the game is over.


# Variables and Constants
For our project, we have used the following data stored in memory

IMMUTABLE DATA:

    Colours:  
        .word 0xff0000 #red  
        .word 0x00ff00 #green  
        .word 0x0000ff #blue  
        .word 0x808080 #grey  
        .word 0xffff00 #yellow  
        .word 0x0 #black   
        .word 0x00FFFF #cyan unbreakable
    
    ADDR_DSPL:
        .word 0x10008000
        
        
    ADDR_KBRD:
        .word 0xffff0000

MUTABLE DATA

    MY_BALL:
        .space 4 #address of the ball
        .word 0xffff00 #colour of the ball
        .word 0x000000 #black colour
        .space 4 #next address of the ball
        
    MY_PADDLE:
        .space 4 #left most address of the paddle
        .word 0xff0000 # color of the paddle
        
    RNG_offset:
        .word 0 #random number

    SCORE:
        .word 0 # one's digit
        .word 0 # ten's digit
        .word 0 # hundred's digit
    
    PLAYER_LIVES:
        .space 4


At the start, we have our immutable data section where we save our colours necessary for our game to function. This allows us to easily access the colour from memory and use constants that are hard to put into registers otherwise (because of the limit on how large the intermediate can be in addi). We want this to be immutable so that the colours which are used for certain functions like checking collisions do not break. We also have the address of the display and keyboard in the immutable data section. 


In the mutable data section, we have the specifications of the ball. This is set as mutable since we need to change the address of the ball as it moves. The paddle is then saved after the ball and similarly holds the address and colour of the paddle. This allows us to move it depending on the keyboard input and thus has to be mutable. RNG\_offset is used to store the direction of the ball and must be changed depending on if a collision occurs. Score updates the score by 10 for every brick the player breaks. Player lives stores how many lives they have left. This is initialized as 3 at the start of the game and then gets reduced by 1 every time the player loses.

# Feature Explanations

### Collision Behaviour
When the ball collides while travelling North East and collide to the wall on the right, it would simply change its direction to the West while keep moving North. Similarly, when the ball collides to the left wall while travelling North West, it would simply change its direction to East while keep moving North. This idea of reflection happens in every collision. This means since our ball initially starts as diagonal movement, it would always go diagonally until the end. There is a random variable that allows for a certain randonmess in direction.

### Pause Feature
When the key p is pressed we enter the a loop that continuously checks for p to be pressed again. Once the key p is pressed again, then we exit the loop and re enter into the game loop.

### Two Paddles
For this feature, we added another check so that if the collision occurs on the third bottom row of the screen, the paddle is not broken (the ball does not paint a black spot).


### Multiple Lives
On each iteration of the game loop, the game checks if it is game over. If it is in fact the end of the game, it checks the player's lives. If player's lives are equal to zero then the game ends. If the player's lives are greater than 0, the paddle and the ball are redrawn and then the game is restarted.


### Speed Change
As explained in the collision behaviour, we added minor random offset which would affect the direction of the ball when it collides against the wall or bricks. Ball will change its speed accordingly as the ball would travel different units depending on the offset.

### Unbreakable Walls
We created an unbreakable wall in the centre coloured as cyan. This wall would never break but just allow the ball to bounce from it.

### Score
Whenever a brick gets destroyed, the score would be incremented by 10. Score is shown on the top right corner of the game display.
