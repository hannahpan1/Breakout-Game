# Breakout-Game
Report of Breakout game coded in MIPS assembly language. Demo upon request.


# Static Image of the Design

<img width="258" alt="breakout-image" src="https://user-images.githubusercontent.com/98304680/211613645-3b62b0d1-c02f-4ab4-93d1-851d5cb97505.png">


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

Mutable Data

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
