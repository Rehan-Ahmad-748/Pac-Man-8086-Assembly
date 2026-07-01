INCLUDE irvine32.inc
INCLUDELIB winmm.lib

PlaySound PROTO,
    pszSound: PTR BYTE,
    hmod:DWORD,
    fdwSound:DWORD

.data
;Bonuses
;1-Audio
;2-Use space to change level
;3-Extra Life

;Audio START
    SND_FILENAME DWORD 00020000h
    SND_LOOP DWORD 00000008h
    SND_ASYNC DWORD 00000001h
    file BYTE "pacman_sound.wav",0
;Audio END

ground BYTE "------------------------------------------------------------------------------------------------------------------------",0
ground1 BYTE "|",0ah,0
ground2 BYTE "|",0

emptySpace BYTE "                                                                                                                        ",0ah,0
pauseText BYTE "GAME PAUSED!",0
resumeText BYTE "Press [p] Again To Resume",0
pauseTextRemove BYTE "            ",0
resumeTextRemove BYTE "                         ",0
tempInput BYTE ?

gamePaused BYTE 0
playerCollision BYTE 0 ;bool to check player collision with wall

;First Screen Start

logo_1 BYTE "      ___           ___           ___           ___           ___           ___     ",0
logo_2 BYTE "     /\  \         /\  \         /\  \         /\__\         /\  \         /\__\    ",0
logo_3 BYTE "    /::\  \       /::\  \       /::\  \       /::|  |       /::\  \       /::|  |   ",0
logo_4 BYTE "   /:/\:\  \     /:/\:\  \     /:/\:\  \     /:|:|  |      /:/\:\  \     /:|:|  |   ",0
logo_5 BYTE "  /::\~\:\  \   /::\~\:\  \   /:/  \:\  \   /:/|:|__|__   /::\~\:\  \   /:/|:|  |__ ",0
logo_6 BYTE " /:/\:\ \:\__\ /:/\:\ \:\__\ /:/__/ \:\__\ /:/ |::::\__\ /:/\:\ \:\__\ /:/ |:| /\__\",0
logo_7 BYTE " \/__\:\/:/  / \/__\:\/:/  / \:\  \  \/__/ \/__/~~/:/  / \/__\:\/:/  / \/__|:|/:/  /",0
logo_8 BYTE "      \::/  /       \::/  /   \:\  \             /:/  /       \::/  /      |:/:/  / ",0
logo_9 BYTE "       \/__/        /:/  /     \:\  \           /:/  /        /:/  /       |::/  /  ",0
logo_10 BYTE"                   /:/  /       \:\__\         /:/  /        /:/  /        /:/  /   ",0
logo_11 BYTE"                   \/__/         \/__/         \/__/         \/__/         \/__/    ",0

press_c BYTE "Press [c] To Continue",0
;First Screen End

;Menu Screen Start
    playButton BYTE "PLAY",0
    instructionButton BYTE "INSTRUCTIONS",0
    highscoreButton BYTE "HIGH SCORES",0
    quitButton BYTE "QUIT",0
    buttonStyle1 BYTE "_",0
    buttonStyle2 BYTE "|",0
    tempButton BYTE ?

    menu_1 BYTE " _      _   ______  _ ",0
    menu_2 BYTE "| \    / | |       | \     | |      |",0
    menu_3 BYTE "|  \  /  | |       |  \    | |      |",0
    menu_4 BYTE "|   \/   | |------ |   \   | |      |",0
    menu_5 BYTE "|        | |       |    \  | |      |",0
    menu_6 BYTE "|        | |______ |     \_| \______/",0

    menuInst BYTE "> Press [p] to play, [i] for instructions, [h] for High Scores and [q] to Quit <",0
    menuChoice BYTE ?
;Menu Screen End

;Instruction Screen Start
    inst_1 BYTE "_____  _         _____  _________  ______             ______  _________ _____  ______   _         _____",0
    inst_2 BYTE "  |   | \     | /     \     |     |      \  |      | /      \     |       |   /      \ | \     | /     \",0
    inst_3 BYTE "  |   |  \    | \_____      |     |      |  |      | |            |       |   |      | |  \    | \_____",0
    inst_4 BYTE "  |   |   \   |       \     |     |______/  |      | |            |       |   |      | |   \   |       \",0
    inst_5 BYTE "  |   |    \  |       |     |     |      \  |      | |            |       |   |      | |    \  |       |",0
    inst_6 BYTE "__|__ |     \_| \_____/     |     |       \ \______/ \______/     |     __|__ \______/ |     \_| \_____/",0

    backButton1 BYTE " ___________",0
    backButton2 BYTE "|           |",0
    backText BYTE "|  BACK[b]  |",0
    backButton3 BYTE "|___________|",0
    backClick BYTE ?

    in_1 BYTE "1- Use W,A,S and D to move Up, Left, Down and Right.",0
    in_2 BYTE "2- Don't bump into ghost, they'll eat your 1 life.",0
    in_3 BYTE "3- Eat coins to earn 1 point.",0
    in_4 BYTE "4- In Level 3, use Teleportation Points to be teleported on the opposite edge.",0
    in_5 BYTE "5- Eat fruits to earn 5 points.",0
    in_6 BYTE "6- Extra life will appear on the map, grab it to gain 1 life.",0

;Instruction Screen End

;Maze Start
    
    Hchar Byte "_",0
    Vchar Byte "|",0
    tempMaze1 Byte ?
    tempMaze2 Byte ?

    MazeX Byte 3000 DUP (0)
    MazeY Byte 3000 DUP (0)
    MazeC Byte 3000 DUP (0)

;Maze End

;Coins
    xPosArr BYTE 1000 DUP (0)
    yPosArr BYTE 1000 DUP (0)
    tempCoin1 Byte ?
    tempCoin2 Byte ?
    coinChar Byte ".",0
;Coins

;Level No. START
    Lvl_1 BYTE ".____                          .__   ",0
    Lvl_2 BYTE "|    |     ____ ___  __  ____  |  |  ",0
    Lvl_3 BYTE "|    |   _/ __ \\  \/ /_/ __ \ |  |  ",0
    Lvl_4 BYTE "|    |___\  ___/ \   / \  ___/ |  |__",0
    Lvl_5 BYTE "|_______ \\___  > \_/   \___  >|____/",0
    Lvl_6 BYTE "        \/    \/            \/       ",0

    one_1 BYTE " ____ ",0
    one_2 BYTE "/_   |",0
    one_3 BYTE " |   |",0
    one_4 BYTE " |   |",0
    one_5 BYTE " |___|",0

    two_1 BYTE "________  ",0
    two_2 BYTE "\_____  \ ",0
    two_3 BYTE " /  ____/ ",0
    two_4 BYTE "/       \ ",0
    two_5 BYTE "\_______ \",0
    two_6 BYTE "        \/",0

    three_1 BYTE "________  ",0
    three_2 BYTE "\_____  \ ",0
    three_3 BYTE "  _(__  < ",0
    three_4 BYTE " /       \",0
    three_5 BYTE "/______  /",0
    three_6 BYTE "       \/ ",0

    xlvl BYTE ?
    ylvl BYTE ?

    levelText BYTE "Level: "
    playerLevel BYTE 1
    levelChange BYTE 0
;Level No. END

;Fruit Start
    fruitTimer BYTE 0
    fruitX BYTE 55
    fruitY BYTE 3
    tempForWall BYTE ?
;Fruit End

;Game Over and End
    go_1 BYTE " ________  ________  _____ ______   _______           ________  ___      ___ _______   ________     ",0
    go_2 BYTE "|\   ____\|\   __  \|\   _ \  _   \|\  ___ \         |\   __  \|\  \    /  /|\  ___ \ |\   __  \    ",0
    go_3 BYTE "\ \  \___|\ \  \|\  \ \  \\\__\ \  \ \   __/|        \ \  \|\  \ \  \  /  / | \   __/|\ \  \|\  \   ",0
    go_4 BYTE "\ \  \  __\ \   __  \ \  \\|__| \  \ \  \_|/__       \ \  \\\  \ \  \/  / / \ \  \_|/_\ \   _  _\  ",0
    go_5 BYTE " \ \  \|\  \ \  \ \  \ \  \    \ \  \ \  \_|\ \       \ \  \\\  \ \    / /   \ \  \_|\ \ \  \\  \| ",0
    go_6 BYTE "  \ \_______\ \__\ \__\ \__\    \ \__\ \_______\       \ \_______\ \__/ /     \ \_______\ \__\\ _\ ",0
    go_7 BYTE "   \|_______|\|__|\|__|\|__|     \|__|\|_______|        \|_______|\|__|/       \|_______|\|__|\|__|",0

    te_1 BYTE " _________  ___  ___  _______           _______   ________   ________     ",0
    te_2 BYTE "|\___   ___\\  \|\  \|\  ___ \         |\  ___ \ |\   ___  \|\   ___ \    ",0
    te_3 BYTE "\|___ \  \_\ \  \\\  \ \   __/|        \ \   __/|\ \  \\ \  \ \  \_|\ \   ",0
    te_4 BYTE "     \ \  \ \ \   __  \ \  \_|/__       \ \  \_|/_\ \  \\ \  \ \  \ \\ \  ",0
    te_5 BYTE "      \ \  \ \ \  \ \  \ \  \_|\ \       \ \  \_|\ \ \  \\ \  \ \  \_\\ \ ",0
    te_6 BYTE "       \ \__\ \ \__\ \__\ \_______\       \ \_______\ \__\\ \__\ \_______\",0
    te_7 BYTE "        \|__|  \|__|\|__|\|_______|        \|_______|\|__| \|__|\|_______|",0

    quit_1 BYTE " ______________",0
    quit_2 BYTE "|              |",0
    quit_3 BYTE "|   QUIT [Q]   |",0
    quit_4 BYTE "|______________|",0

    pa_1 BYTE " ___________________",0
    pa_2 BYTE "|                   |",0
    pa_3 BYTE "|    RESTART [R]    |",0
    pa_4 BYTE "|___________________|",0

    scorePrompt BYTE "'s Score: ",0

    xGO BYTE ?
    yGO BYTE ?

    gameEndChoice BYTE ?
;Game Over and End



temp byte ?

;Player START
strScore BYTE "Your score is: ",0
score Word 0

strLives BYTE "Remaining Lives: ",0
lives BYTE 3

xPos BYTE 100
yPos BYTE 4

playerMoveLeft Byte 1
playerMoveRight Byte 0
playerMoveUp Byte 0
playerMoveDown Byte 0

playerName BYTE ?

enterName BYTE "ENTER YOUR NAME :)",0
playerBox1 BYTE "_____________________________",0
playerBox2 BYTE "|                             |",0
playerBox3 BYTE "|                             |",0
playerBox4 BYTE "|_____________________________|",0
;Player END

;GHOST START
    xGhost_1 BYTE 21
    yGhost_1 BYTE 5

    xGhost_2 BYTE 0
    yGhost_2 BYTE 0

    xGhost_3 BYTE 0
    yGhost_3 BYTE 0

    mUp BYTE 0
    mDown BYTE 0
    mRight BYTE 0
    mLeft BYTE 0

    tempX BYTE ?
    tempY BYTE ?

    GhostTimer_1 BYTE 14
    GhostTimer_2 BYTE 14
    GhostTimer_3 BYTE 14

    lastMove_1 DD ?
    lastMove_2 DD ?
    lastMove_3 DD ?
;GHOST END

;Extra Life
    Xlife Byte ?
    Ylife Byte ?
    lifeStart Byte 0
    lifeEnd Byte 0
;Extra Life

;Invisible
    Xinv Byte ?
    Yinv Byte ?
    invStart Byte 0
    invEnd Byte 0

    pInv Byte 0
;Invisible

xCoinPos BYTE ?
yCoinPos BYTE ?

inputChar BYTE ?
charBuffer BYTE ?

;File Handling Start

    filename Byte "scores.txt"
    filehandle DD ?
    stringLength DD ?

;File Handling End
.code

main PROC

    mov edx,OFFSET filename
    call CreateOutputFile
    mov filehandle, EAX

    mov eax, SND_ASYNC
    invoke PlaySound, addr file , 0 ,eax

    call DrawPacManLogo
    mov eax,fileHandle
    mov edx,OFFSET playerName
    mov ecx,stringLength
    call WriteToFile

    mov eax,fileHandle
    call CloseFile

    menuAgain:
    call DrawMenu

    call readchar
    mov menuChoice,al

    cmp menuChoice,"p"
    je play
    cmp menuChoice,"i"
    je instructionScreen
    cmp menuChoice,"h"
    je exitGame
    cmp menuChoice,"q"
    je exitGame

    jmp menuAgain

    instructionScreen:
    call CleanScreen
    call DrawInstructionScreen
    jmp menuAgain

    play:

    mov gameEndChoice,"o"; just a garbage value
    call GameOver
        cmp gameEndChoice,"q"
        je exitGame
        cmp gameEndChoice,"r"
        je play
    mov lives, 3
    mov levelChange,0
    call CleanScreen
    call DrawLevelLogo

    cmp playerLevel,1
    jne setCharac_1

    mov xGhost_1,21
    mov yGhost_1,5

    jmp continueDraw
    setCharac_1:
    cmp playerLevel,2
    jne setCharac_2

    mov xGhost_1,53
    mov yGhost_1,4

    jmp continueDraw
    setCharac_2:
    cmp playerLevel,3
    jne continueDraw

    mov xGhost_1,10
    mov yGhost_1,27

    mov xGhost_2,55
    mov yGhost_2,27

    mov xGhost_3,110
    mov yGhost_3,27

    continueDraw:
    mov eax,blue ;(black * 16)
    call SetTextColor
    
    call DrawPlayer

    call Randomize
    call DrawLevel
    call DrawCoins
    

    gameLoop:
        
        mov al,inputChar
        mov tempInput,al

        endPause:
        call InvisibleChar
        call ExtraLife
        call GhostCollision
        call Ghost
        call GhostCollision
        call GameOver
        cmp gameEndChoice,"q"
        je exitGame
        cmp gameEndChoice,"r"
        je play

        call Teleportation
        call DrawLevelText
        call CoinPoints
        call LevelCheck
        mov al,levelChange
        cmp al,1
        je play

        inc fruitTimer
        cmp fruitTimer,5
        jb no_fruit
        call DrawFruit

        no_fruit:

        mov eax,120
        call Delay
       
        ;Reset Player Wall Collision Bool
        mov playerCollision,0


        ;Reset Pause Settings
        mov gamePaused,0


        mov eax,white (black * 16)
        call SetTextColor

        ; draw score:
        mov dl,0
        mov dh,0
        call Gotoxy
        mov edx,OFFSET strScore
        call WriteString
        mov ax,score
        call writedec

        mov eax,white (black * 16)
        call SetTextColor

        ; draw lives:
        mov dl,100
        mov dh,0
        call Gotoxy
        mov edx,OFFSET strLives
        call WriteString
        mov al,lives
        call writedec

        ;Retrieve last input value
        mov al,tempInput
        mov inputChar, al

        ; get user key input:
        getInputFromUser:

        cmp inputChar, "p"
        jne continue_input

        until_p:
        call ReadChar
        mov inputChar,al
        cmp inputChar, "p"
        jne until_p

        call RemovePauseText
        jmp endPause

        continue_input:
         mov eax,0
        call ReadKey

        ;if user doesn't give input player will continue its movement
        cmp eax,1
        je NoInput_Continue
        mov inputChar,al

        NoInput_Continue:

        

        continueTheGame:

        ; exit game if user types 'x':
        cmp inputChar,"x"
        je exitGame

        ; pause game if user types 'p':
        cmp inputChar,"p"
        je pauseTheGame

        cmp inputChar,"w"
        je moveUp

        cmp inputChar,"s"
        je moveDown

        cmp inputChar,"a"
        je moveLeft

        cmp inputChar,"d"
        je moveRight

        cmp inputChar, " "
        je levelHack

        ;incase no valid key pressed gameloop will continue
        jmp endPause

        pauseTheGame:
        mov gamePaused,1
        mov eax, yellow
        call SetTextColor

        mov dl,55
        mov dh,0
        call Gotoxy
        mov edx,OFFSET pauseText
        call WriteString

        mov dl,48
        mov dh,1
        call Gotoxy
        mov edx,OFFSET resumeText
        call WriteString
        
        jmp getInputFromUser

        moveUp:
        dec yPos
        call PlayerWallCollision
        inc yPos
        mov al,playerCollision
        cmp al,1
        je gameLoop

        call UpdatePlayer
        dec yPos
        call DrawPlayer
        jmp gameLoop

        moveDown:
        inc yPos
        call PlayerWallCollision
        dec yPos
        mov al,playerCollision
        cmp al,1
        je gameLoop

        call UpdatePlayer
        inc yPos
        call DrawPlayer
        jmp gameLoop

        moveLeft:
        dec xPos
        call PlayerWallCollision
        inc xPos
        mov al,playerCollision
        cmp al,1
        je gameLoop
        
        call UpdatePlayer
        dec xPos
        call DrawPlayer
        jmp gameLoop

        moveRight:
        inc xPos
        call PlayerWallCollision
        dec xPos
        mov al,playerCollision
        cmp al,1
        je gameLoop

        call UpdatePlayer
        inc xPos
        call DrawPlayer
        jmp gameLoop

        levelHack:
        call ChangeLevel
        mov al,tempInput
        mov inputChar,al
        jmp endPause
    
    
    jmp gameLoop

    exitGame:
    call CleanScreen
    exit
main ENDP

DrawPlayer PROC

    cmp pInv,0
    jnbe setInvisible
    ; draw player at (xPos,yPos):
    mov eax,yellow ;(blue*16)
    call SetTextColor
    jmp setGreen

    setInvisible:
    mov eax,lightcyan
    call SetTextColor
    
    setGreen:
    mov dl,xPos
    mov dh,yPos
    call Gotoxy
    mov al,"X"
    call WriteChar

    ret
DrawPlayer ENDP

UpdatePlayer PROC
    
    cmp pInv,0
    jnbe setInvisible

    mov dl,xPos
    mov dh,yPos
    call Gotoxy
    mov al," "
    call WriteChar
    jmp exitUpdate

    setInvisible:
    mov dl,xPos
    mov dh,yPos
    call Gotoxy
    mov al," "
    call WriteChar

    mov dl,xPos
    mov dh,yPos
    mov eax,0
    mov ecx,3000
    WallCheck:
        cmp MazeX[eax],dl
        jne next_itt
        cmp MazeY[eax],dh
        jne next_itt

        mov al,MazeC[eax]
        mov tempForWall,al
        mov eax,green
        call settextcolor
        mov dl,xPos
        mov dh,yPos
        call gotoxy
        mov al,tempForWall
        call WriteChar
        jmp exitUpdate

        next_itt:
        inc eax
    Loop WallCheck

    exitUpdate:
    ret
UpdatePlayer ENDP

DrawEnemy PROC
    ; draw enemy at (xPosEnemy,yPosEnemy):
    mov eax,red ;(blue*16)
    call SetTextColor
    

    call Gotoxy
    mov al,"O"
    call WriteChar
    ret
DrawEnemy ENDP

UpdateEnemy PROC
    
    call Gotoxy
    mov al," "
    call WriteChar
    ret
UpdateEnemy ENDP

ReDrawCoin PROC
    
    mov tempX,dl
    mov tempY,dh
    mov eax,0
    mov ecx,1000
    L1:
        cmp xPosArr[eax],dl
        jne next_itteration
        cmp yPosArr[eax],dh
        jne next_itteration

        mov eax,yellow
        call settextcolor
        mov dl,tempX
        mov dh,tempY
        call gotoxy
        mov al,"."
        call WriteChar

        jmp exitDraw

        next_itteration:
        inc eax
    Loop L1

    exitDraw:
    ret
ReDrawCoin ENDP

ChangeLevel PROC
    mov eax,0
    mov ecx,1000
    L1:
        mov xPosArr[eax],0
        mov yPosArr[eax],0

        inc eax
    Loop L1
    
    ret
ChangeLevel ENDP

GhostCollisionWall PROC
    
    mov tempX,dl
    mov tempY,dh
    mov eax,0
    mov ecx,3000
    L1:
        cmp MazeX[eax],dl
        jne next_itteration
        cmp MazeY[eax],dh
        jne next_itteration

        mov eax,4000
        mov dl,tempX
        mov dh,tempY

        jmp exitCheck

        next_itteration:
        inc eax
    Loop L1

    exitCheck:
    ret
GhostCollisionWall ENDP

GhostCollision PROC
    
    mov dl,xPos
    mov dh,yPos

    cmp dl,xGhost_1
    jne check_1
    cmp dh,yGhost_1
    jne check_1

    dec lives
    mov xPos,55
    mov yPos,10

    check_1:
    cmp dl,xGhost_2
    jne check_2
    cmp dh,yGhost_2
    jne check_2

    dec lives
    mov xPos,55
    mov yPos,10

    check_2:
    cmp dl,xGhost_3
    jne exitCheck
    cmp dh,yGhost_3
    jne exitCheck

    dec lives
    mov xPos,55
    mov yPos,10

    exitCheck:
    ret
GhostCollision ENDP

Ghost PROC
    
    mov dl,xGhost_1
    mov dh,yGhost_1
    call UpdateEnemy
    mov dl,xGhost_1
    mov dh,yGhost_1
    call ReDrawCoin

    cmp playerLevel,1
    je L1Ghost
    cmp playerLevel,2
    je L2Ghost
    cmp playerLevel,3
    je L3Ghost

    L1Ghost:
    cmp xGhost_1,21
    jne next_check_1
    cmp yGhost_1,5 
    jne next_check_1

    mov mLeft,0
    mov mRight,1
    mov mUp,0
    mov mDown,0

    next_check_1:
    cmp xGhost_1,101
    jne next_check_2
    cmp yGhost_1,5
    jne next_check_2

    mov mLeft,0
    mov mRight,0
    mov mUp,0
    mov mDown,1

    next_check_2:
    cmp xGhost_1,101
    jne next_check_3
    cmp yGhost_1,27
    jne next_check_3

    mov mLeft,1
    mov mRight,0
    mov mUp,0
    mov mDown,0

    next_check_3:
    cmp xGhost_1,21
    jne startMove
    cmp yGhost_1,27
    jne startMove

    mov mLeft,0
    mov mRight,0
    mov mUp,1
    mov mDown,0

    startMove:
    cmp mUp,1
    je ghostUp
    cmp mDown,1
    je ghostDown
    cmp mLeft,1
    je ghostLeft
    cmp mRight,1
    je ghostRight


    ghostUp:
    dec yGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp exitGhost

    ghostDown:
    inc yGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp exitGhost

    ghostLeft:
    dec xGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp exitGhost

    ghostRight:
    inc xGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy

    jmp exitGhost
    L2Ghost:

    cmp xGhost_1,53
    jne check_1
    cmp yGhost_1,4 
    jne check_1

    mov mLeft,0
    mov mRight,0
    mov mUp,0
    mov mDown,1

    check_1:
    cmp xGhost_1,53
    jne check_2
    cmp yGhost_1,15
    jne check_2

    mov mLeft,1
    mov mRight,0
    mov mUp,0
    mov mDown,0

    check_2:
    cmp xGhost_1,14
    jne check_3
    cmp yGhost_1,15
    jne check_3

    mov mLeft,0
    mov mRight,0
    mov mUp,0
    mov mDown,1

    check_3:
    cmp xGhost_1,14
    jne check_4
    cmp yGhost_1,27
    jne check_4

    mov mLeft,0
    mov mRight,1
    mov mUp,0
    mov mDown,0

    check_4:
    cmp xGhost_1,105
    jne check_5
    cmp yGhost_1,27
    jne check_5

    mov mLeft,0
    mov mRight,0
    mov mUp,1
    mov mDown,0

    check_5:
    cmp xGhost_1,105
    jne check_6
    cmp yGhost_1,15
    jne check_6

    mov mLeft,1
    mov mRight,0
    mov mUp,0
    mov mDown,0

    check_6:
    cmp xGhost_1,71
    jne check_7
    cmp yGhost_1,15
    jne check_7

    mov mLeft,0
    mov mRight,0
    mov mUp,1
    mov mDown,0

    check_7:
    cmp xGhost_1,71
    jne startMove_2
    cmp yGhost_1,4
    jne startMove_2

    mov mLeft,1
    mov mRight,0
    mov mUp,0
    mov mDown,0


    startMove_2:
    cmp mUp,1
    je ghostUp
    cmp mDown,1
    je ghostDown
    cmp mLeft,1
    je ghostLeft
    cmp mRight,1
    je ghostRight


    ghostUp_2:
    dec yGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp exitGhost

    ghostDown_2:
    inc yGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp exitGhost

    ghostLeft_2:
    dec xGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp exitGhost

    ghostRight_2:
    inc xGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy

    jmp exitGhost
    L3Ghost:

    mov dl,xGhost_1
    mov dh,yGhost_1
    call UpdateEnemy
    mov dl,xGhost_1
    mov dh,yGhost_1
    call ReDrawCoin

    mov dl,xGhost_2
    mov dh,yGhost_2
    call UpdateEnemy
    mov dl,xGhost_2
    mov dh,yGhost_2
    call ReDrawCoin

    mov dl,xGhost_3
    mov dh,yGhost_3
    call UpdateEnemy
    mov dl,xGhost_3
    mov dh,yGhost_3
    call ReDrawCoin

    inc GhostTimer_1
    cmp GhostTimer_1,15
    jne moveGhost_1

    againRandom_1:
    mov GhostTimer_1,0

    mov eax,4
    call RandomRange
    mov lastMove_1,eax

    moveGhost_1:
    mov eax,lastMove_1
    cmp eax,0
    je setLeft_1
    cmp eax,1
    je setRight_1
    cmp eax,3
    je setUp_1
    cmp eax,2
    je setDown_1

    setLeft_1:
    dec xGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call GhostCollisionWall
    inc xGhost_1
    cmp eax,4000
    je againRandom_1
    dec xGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp Ghost_2

    setRight_1:
    inc xGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call GhostCollisionWall
    dec xGhost_1
    cmp eax,4000
    je againRandom_1
    inc xGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp Ghost_2

    setUp_1:
    dec yGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call GhostCollisionWall
    inc yGhost_1
    cmp eax,4000
    je againRandom_1
    dec yGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp Ghost_2

    setDown_1:
    inc yGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call GhostCollisionWall
    dec yGhost_1
    cmp eax,4000
    je againRandom_1
    inc yGhost_1
    mov dl,xGhost_1
    mov dh,yGhost_1
    call DrawEnemy
    jmp Ghost_2



    Ghost_2:
    inc GhostTimer_2
    cmp GhostTimer_2,15
    jne moveGhost_2

    againRandom_2:
    mov GhostTimer_2,0

    mov eax,4
    call RandomRange
    mov lastMove_2,eax

    moveGhost_2:
    mov eax,lastMove_2
    cmp eax,0
    je setLeft_2
    cmp eax,1
    je setRight_2
    cmp eax,3
    je setUp_2
    cmp eax,2
    je setDown_2

    setLeft_2:
    dec xGhost_2
    mov dl,xGhost_2
    mov dh,yGhost_2
    call GhostCollisionWall
    inc xGhost_2
    cmp eax,4000
    je againRandom_2
    dec xGhost_2
    mov dl,xGhost_2
    mov dh,yGhost_2
    call DrawEnemy
    jmp Ghost_3

    setRight_2:
    inc xGhost_2
    mov dl,xGhost_2
    mov dh,yGhost_2
    call GhostCollisionWall
    dec xGhost_2
    cmp eax,4000
    je againRandom_2
    inc xGhost_2
    mov dl,xGhost_2
    mov dh,yGhost_2
    call DrawEnemy
    jmp Ghost_3

    setUp_2:
    dec yGhost_2
    mov dl,xGhost_2
    mov dh,yGhost_2
    call GhostCollisionWall
    inc yGhost_2
    cmp eax,4000
    je againRandom_2
    dec yGhost_2
    mov dl,xGhost_2
    mov dh,yGhost_2
    call DrawEnemy
    jmp Ghost_3

    setDown_2:
    inc yGhost_2
    mov dl,xGhost_2
    mov dh,yGhost_2
    call GhostCollisionWall
    dec yGhost_2
    cmp eax,4000
    je againRandom_2
    inc yGhost_2
    mov dl,xGhost_2
    mov dh,yGhost_2
    call DrawEnemy
    jmp Ghost_3

    Ghost_3:
    inc GhostTimer_3
    cmp GhostTimer_3,15
    jne moveGhost_3

    againRandom_3:
    mov GhostTimer_3,0

    mov eax,4
    call RandomRange
    mov lastMove_3,eax

    moveGhost_3:
    mov eax,lastMove_3
    cmp eax,0
    je setLeft_3
    cmp eax,1
    je setRight_3
    cmp eax,3
    je setUp_3
    cmp eax,2
    je setDown_3

    setLeft_3:
    dec xGhost_3
    mov dl,xGhost_3
    mov dh,yGhost_3
    call GhostCollisionWall
    inc xGhost_3
    cmp eax,4000
    je againRandom_3
    dec xGhost_3
    mov dl,xGhost_3
    mov dh,yGhost_3
    call DrawEnemy
    jmp Ghost_4

    setRight_3:
    inc xGhost_3
    mov dl,xGhost_3
    mov dh,yGhost_3
    call GhostCollisionWall
    dec xGhost_3
    cmp eax,4000
    je againRandom_3
    inc xGhost_3
    mov dl,xGhost_3
    mov dh,yGhost_3
    call DrawEnemy
    jmp Ghost_4

    setUp_3:
    dec yGhost_3
    mov dl,xGhost_3
    mov dh,yGhost_3
    call GhostCollisionWall
    inc yGhost_3
    cmp eax,4000
    je againRandom_3
    dec yGhost_3
    mov dl,xGhost_3
    mov dh,yGhost_3
    call DrawEnemy
    jmp Ghost_4

    setDown_3:
    inc yGhost_3
    mov dl,xGhost_3
    mov dh,yGhost_3
    call GhostCollisionWall
    dec yGhost_3
    cmp eax,4000
    je againRandom_3
    inc yGhost_3
    mov dl,xGhost_3
    mov dh,yGhost_3
    call DrawEnemy
    jmp Ghost_4

    Ghost_4:



    exitGhost:
    ret
Ghost ENDP

CleanScreen PROC
    mov dl,0
    mov dh,0
    mov ecx,30
    L1:
        call Gotoxy
        mov edx,OFFSET emptySpace
        call WriteString
        inc dh
    Loop L1
    ret
CleanScreen ENDP

PlayerWallCollision PROC
    mov dl,xPos
    mov dh,yPos

    cmp dl,0
    je playerCollided

    cmp dl,119
    je playerCollided

    cmp dh,2
    je playerCollided

    cmp dh,29
    je playerCollided

    cmp pInv,0
    jnbe setInvisible

    mov eax,0
    mov ecx,3000
    CollisionCheck:
        cmp MazeX[eax],dl
        jne next_itteration
        cmp MazeY[eax],dh
        je playerCollided

        next_itteration:
        inc eax
    Loop CollisionCheck

    jmp exitCollisionChecker

    playerCollided:
    mov eax,0
    mov al,1
    mov playerCollision,al
    jmp exitCollisionChecker

    setInvisible:
    dec pInv


    exitCollisionChecker:
    ret
PlayerWallCollision ENDP

DrawLevel PROC
    mov eax, 10
    call settextcolor

    mov eax,0

    mov dl,0
    mov tempMaze1,dl
    mov dh,2
    mov tempMaze2,dh
    
    mov ecx, 119
    UP:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop UP

    mov dl,0
    mov tempMaze1,dl
    mov dh,28
    mov tempMaze2,dh
    
    mov ecx, 119
    DOWN:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop DOWN

    mov dl,0
    mov tempMaze1,dl
    mov dh,3
    mov tempMaze2,dh
    
    mov ecx, 26
    LEFT:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop LEFT

    mov dl,119
    mov tempMaze1,dl
    mov dh,3
    mov tempMaze2,dh
    
    mov ecx, 26
    RIGHT:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop RIGHT

    mov dl,24
    mov tempMaze1,dl
    mov dh,6
    mov tempMaze2,dh
    
    mov ecx, 21
    L1:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L1

    mov dl,78
    mov tempMaze1,dl
    mov dh,6
    mov tempMaze2,dh
    
    mov ecx, 21
    L2:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L2

    mov dl,78
    mov tempMaze1,dl
    mov dh,25
    mov tempMaze2,dh
    
    mov ecx, 21
    L3:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L3

    mov dl,24
    mov tempMaze1,dl
    mov dh,25
    mov tempMaze2,dh
    
    mov ecx, 20
    L4:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L4

    mov dl,44
    mov tempMaze1,dl
    mov dh,7
    mov tempMaze2,dh
    
    mov ecx, 7
    L5:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop L5

    mov dl,78
    mov tempMaze1,dl
    mov dh,7
    mov tempMaze2,dh
    
    mov ecx, 7
    L6:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop L6

    mov dl,78
    mov tempMaze1,dl
    mov dh,25
    mov tempMaze2,dh
    
    mov ecx, 7
    L7:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        dec tempMaze2
    Loop L7

    mov dl,44
    mov tempMaze1,dl
    mov dh,25
    mov tempMaze2,dh
    
    mov ecx, 7
    L8:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        dec tempMaze2
    Loop L8

    mov dl,24
    mov tempMaze1,dl
    mov dh,13
    mov tempMaze2,dh
    
    mov ecx, 20
    L9:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L9

    mov dl,79
    mov tempMaze1,dl
    mov dh,13
    mov tempMaze2,dh
    
    mov ecx, 20
    L10:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L10

    mov dl,78
    mov tempMaze1,dl
    mov dh,18
    mov tempMaze2,dh
    
    mov ecx, 21
    L11:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L11

    mov dl,24
    mov tempMaze1,dl
    mov dh,18
    mov tempMaze2,dh
    
    mov ecx, 21
    L12:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L12





    mov bl,0
    mov bl,playerLevel
    cmp bl,1
    je exitMazeCreation
    ;Level 2

    mov dl,46
    mov tempMaze1,dl
    mov dh,16
    mov tempMaze2,dh
    
    mov ecx, 30
    L13:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L13

    mov dl,61
    mov tempMaze1,dl
    mov dh,7
    mov tempMaze2,dh
    
    mov ecx, 19
    L14:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop L14




    mov bl,0
    mov bl,playerLevel
    cmp bl,2
    je exitMazeCreation
    ;Level 3
    mov dl,13
    mov tempMaze1,dl
    mov dh,9
    mov tempMaze2,dh
    
    mov ecx, 12
    L15:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L15

    mov dl,13
    mov tempMaze1,dl
    mov dh,22
    mov tempMaze2,dh
    
    mov ecx, 12
    L16:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L16

    mov dl,12
    mov tempMaze1,dl
    mov dh,10
    mov tempMaze2,dh
    
    mov ecx, 13
    L17:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop L17

    mov dl,98
    mov tempMaze1,dl
    mov dh,9
    mov tempMaze2,dh
    
    mov ecx, 12
    L18:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L18

    mov dl,98
    mov tempMaze1,dl
    mov dh,22
    mov tempMaze2,dh
    
    mov ecx, 12
    L19:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L19

    mov dl,110
    mov tempMaze1,dl
    mov dh,10
    mov tempMaze2,dh
    
    mov ecx, 13
    L20:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop L20

    ;Teleportation

    mov dl,2
    mov tempMaze1,dl
    mov dh,25
    mov tempMaze2,dh
    
    mov ecx, 3
    L21:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop L21

    mov dl,3
    mov tempMaze1,dl
    mov dh,27
    mov tempMaze2,dh
    
    mov ecx, 5
    L22:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L22

    mov dl,117
    mov tempMaze1,dl
    mov dh,4
    mov tempMaze2,dh
    
    mov ecx, 3
    L23:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],124
        inc eax

        mov edx, offset Vchar
        call writestring

        inc tempMaze2
    Loop L23

    mov dl,112
    mov tempMaze1,dl
    mov dh,3
    mov tempMaze2,dh
    
    mov ecx, 5
    L24:
        mov dl, tempMaze1
        mov dh, tempMaze2
        call gotoxy

        mov MazeX[eax],dl
        mov MazeY[eax],dh
        mov MazeC[eax],"_"
        inc eax

        mov edx, offset Hchar
        call writestring

        inc tempMaze1
    Loop L24

    exitMazeCreation:
    mov dl,0
    mov dh,0
    call gotoxy

    ret
DrawLevel ENDP

DrawCoins PROC
    mov eax,yellow
    call settextcolor

    mov edi, offset xPosArr
    mov esi, offset yPosArr
    mov dl,2
    mov tempCoin1, dl
    mov dh,3
    mov tempCoin2, dh

    L1:
    mov tempCoin1,2  
        L2:
        mov dl, tempCoin1
        mov dh, tempCoin2

        mov ecx,3000
        mov eax,0
        CheckWall:
            cmp MazeX[eax],dl
            jne next_itteration
            cmp MazeY[eax],dh
            je NoCoin

            next_itteration:
            inc eax
        Loop CheckWall

        mov [edi],dl
        mov [esi],dh
        call gotoxy
        mov edx, offset coinChar
        call writestring
       
        inc edi
        inc esi

        NoCoin:
        add tempCoin1,8
        mov dl, tempCoin1
        cmp dl,119
        jb L2

    add tempCoin2,4
    mov dh,tempCoin2
    cmp dh,29
    jb L1

    ret
DrawCoins ENDP

CoinPoints PROC
    mov dl,xPos
    mov dh,yPos

    mov eax,0
    mov ecx,1000
    L1:
        cmp dl,xPosArr[eax]
        jne next_itteration
        cmp dh,yPosArr[eax]
        je coin_found

        next_itteration:
        inc eax
    Loop L1
    jmp exitPoint

    
    coin_found:
    inc score
    mov xPosArr[eax],0
    mov yPosArr[eax],0

    exitPoint:
    ret
CoinPoints ENDP

DrawPacManLogo PROC
    mov eax, brown
    call SetTextColor



    mov dl,17
    mov dh,5
    call Gotoxy
    mov edx,OFFSET logo_1
    call WriteString

    mov dl,17
    mov dh,6
    call Gotoxy
    mov edx,OFFSET logo_2
    call WriteString

    mov dl,17
    mov dh,7
    call Gotoxy
    mov edx,OFFSET logo_3
    call WriteString

    mov dl,17
    mov dh,8
    call Gotoxy
    mov edx,OFFSET logo_4
    call WriteString

    mov dl,17
    mov dh,9
    call Gotoxy
    mov edx,OFFSET logo_5
    call WriteString

    mov dl,17
    mov dh,10
    call Gotoxy
    mov edx,OFFSET logo_6
    call WriteString

    mov dl,17
    mov dh,11
    call Gotoxy
    mov edx,OFFSET logo_7
    call WriteString

    mov dl,17
    mov dh,12
    call Gotoxy
    mov edx,OFFSET logo_8
    call WriteString

    mov dl,17
    mov dh,13
    call Gotoxy
    mov edx,OFFSET logo_9
    call WriteString

    mov dl,17
    mov dh,14
    call Gotoxy
    mov edx,OFFSET logo_10
    call WriteString

    mov dl,17
    mov dh,15
    call Gotoxy
    mov edx,OFFSET logo_11
    call WriteString



    mov eax, cyan
    call SetTextColor


    mov dl,43
    mov dh,20
    call Gotoxy
    mov edx, offset playerBox1
    call WriteString

    mov dl,42
    mov dh,23
    call Gotoxy
    mov edx, offset playerBox4
    call WriteString

    mov dl,42
    mov dh,21
    call Gotoxy
    mov edx, offset playerBox2
    call WriteString

    mov dl,42
    mov dh,22
    call Gotoxy
    mov edx, offset playerBox3
    call WriteString
    


    mov eax, brown
    call SetTextColor

    mov dl,48
    mov dh,19
    call Gotoxy
    mov edx, offset enterName
    call WriteString

    mov dl,46
    mov dh,22
    call gotoxy
    mov edx, offset playerName
    mov ecx, 255
    call ReadString
    mov stringLength,eax

    call CleanScreen
    ret
DrawPacManLogo ENDP

DrawMenu PROC

    mov eax, brown
    call settextcolor
    mov dl,39
    mov dh,0
    call Gotoxy
    mov edx, offset menu_1
    call writestring

    mov dl,39
    mov dh,1
    call Gotoxy
    mov edx, offset menu_2
    call writestring

    mov dl,39
    mov dh,2
    call Gotoxy
    mov edx, offset menu_3
    call writestring

    mov dl,39
    mov dh,3
    call Gotoxy
    mov edx, offset menu_4
    call writestring

    mov dl,39
    mov dh,4
    call Gotoxy
    mov edx, offset menu_5
    call writestring

    mov dl,39
    mov dh,5
    call Gotoxy
    mov edx, offset menu_6
    call writestring

    mov eax, cyan
    call settextcolor

    mov dl,56
    mov dh,9
    call Gotoxy
    mov edx, offset playButton
    call writestring

    mov dl,52
    mov dh,13
    call Gotoxy
    mov edx, offset instructionButton
    call writestring

    mov dl,52
    mov dh,17
    call Gotoxy
    mov edx, offset highscoreButton
    call writestring

    mov dl,56
    mov dh,21
    call Gotoxy
    mov edx, offset quitButton
    call writestring

    mov dl,49
    mov dh,7
    mov ecx,18
    L1:
    call Gotoxy
    mov edx, offset buttonStyle1
    call writestring
    inc dl
    Loop L1

    mov dl,49
    mov dh,10
    mov ecx,18
    L2:
    call Gotoxy
    mov edx, offset buttonStyle1
    call writestring
    inc dl
    Loop L2
    
    mov dl,49
    mov dh,11
    mov ecx,18
    L3:
    call Gotoxy
    mov edx, offset buttonStyle1
    call writestring
    inc dl
    Loop L3

    mov dl,49
    mov dh,14
    mov ecx,18
    L4:
    call Gotoxy
    mov edx, offset buttonStyle1
    call writestring
    inc dl
    Loop L4

    mov dl,49
    mov dh,15
    mov ecx,18
    L5:
    call Gotoxy
    mov edx, offset buttonStyle1
    call writestring
    inc dl
    Loop L5

    mov dl,49
    mov dh,18
    mov ecx,18
    L6:
    call Gotoxy
    mov edx, offset buttonStyle1
    call writestring
    inc dl
    Loop L6
    
    mov dl,49
    mov dh,19
    mov ecx,18
    L7:
    call Gotoxy
    mov edx, offset buttonStyle1
    call writestring
    inc dl
    Loop L7

    mov dl,49
    mov dh,22
    mov ecx,18
    L8:
    call Gotoxy
    mov edx, offset buttonStyle1
    call writestring
    inc dl
    Loop L8

    mov tempButton,8
    mov ecx,15

    side_L1:
        mov dl,48
        mov dh,tempButton

        cmp dh,11
        je dontDisplay
        cmp dh,15
        je dontDisplay
        cmp dh,19
        je dontDisplay

        call Gotoxy
        mov edx, offset buttonStyle2
        call writestring

        dontDisplay:
        inc tempButton
    Loop side_L1

    mov tempButton,8
    mov ecx,15

    side_L2:
        mov dl,67
        mov dh,tempButton

        cmp dh,11
        je dontDisplay2
        cmp dh,15
        je dontDisplay2
        cmp dh,19
        je dontDisplay2

        call Gotoxy
        mov edx, offset buttonStyle2
        call writestring

        dontDisplay2:
        inc tempButton
    Loop side_L2

    mov eax, gray
    call settextcolor
    mov dl,21
    mov dh,27
    call Gotoxy
    mov edx, offset menuInst
    call writestring

    ret
DrawMenu ENDP

DrawInstructionScreen PROC
    mov eax,brown
    call settextcolor

    mov dl,7
    mov dh,0
    call Gotoxy
    mov edx, offset inst_1
    call writestring

    mov dl,7
    mov dh,1
    call Gotoxy
    mov edx, offset inst_2
    call writestring

    mov dl,7
    mov dh,2
    call Gotoxy
    mov edx, offset inst_3
    call writestring

    mov dl,7
    mov dh,3
    call Gotoxy
    mov edx, offset inst_4
    call writestring

    mov dl,7
    mov dh,4
    call Gotoxy
    mov edx, offset inst_5
    call writestring

    mov dl,7
    mov dh,5
    call Gotoxy
    mov edx, offset inst_6
    call writestring

    mov eax,cyan
    call settextcolor

    mov dl,20
    mov dh,13
    call Gotoxy
    mov edx, offset in_1
    call writestring

    mov dl,20
    mov dh,14
    call Gotoxy
    mov edx, offset in_2
    call writestring

    mov dl,20
    mov dh,15
    call Gotoxy
    mov edx, offset in_3
    call writestring

    mov dl,20
    mov dh,16
    call Gotoxy
    mov edx, offset in_4
    call writestring

    mov dl,20
    mov dh,17
    call Gotoxy
    mov edx, offset in_5
    call writestring

    mov dl,20
    mov dh,18
    call Gotoxy
    mov edx, offset in_6
    call writestring

    mov dl,106
    mov dh,27
    call Gotoxy
    mov edx, offset backText
    call writestring

    mov dl,106
    mov dh,25
    call Gotoxy
    mov edx, offset backButton1
    call writestring

    mov dl,106
    mov dh,26
    call Gotoxy
    mov edx, offset backButton2
    call writestring

    mov dl,106
    mov dh,28
    call Gotoxy
    mov edx, offset backButton3
    call writestring

    backAgain:
    call readchar
    mov backClick,al
    cmp backClick,"b"
    jne backAgain
    

    call CleanScreen

    ret
DrawInstructionScreen ENDP

RemovePauseText PROC
        mov dl,55
        mov dh,0
        call Gotoxy
        mov edx,OFFSET pauseTextRemove
        call WriteString

        mov dl,48
        mov dh,1
        call Gotoxy
        mov edx,OFFSET resumeTextRemove
        call WriteString

        ret
RemovePauseText ENDP

DrawLevelText PROC
    mov eax,white
    call settextcolor

    mov dl,0
    mov dh,1
    call gotoxy

    mov edx, offset levelText
    call writestring
    
    mov eax,0
    mov al,playerLevel
    call WriteDec

    ret
DrawLevelText ENDP

LevelCheck PROC
    mov dl,0
    mov eax,0
    mov ecx,100
    L1:
        cmp xPosArr[eax],dl
        jne exitLevelCheck
        cmp yPosArr[eax],dl
        jne exitLevelCheck

        inc eax
    Loop L1
    inc playerLevel
    mov levelChange,1

    exitLevelCheck:
    ret
LevelCheck ENDP

DrawFruit PROC

    cmp playerLevel,1
    je exitFruit

    mov dl,fruitX
    mov dh,fruitY

    mov eax,0
    mov ecx,3000
    WallCheck:
        cmp MazeX[eax],dl
        jne next_itt
        cmp MazeY[eax],dh
        jne next_itt

        mov al,MazeC[eax]
        mov tempForWall,al
        mov eax,green
        call settextcolor
        mov dl,fruitX
        mov dh,fruitY
        call gotoxy
        mov al,tempForWall
        call WriteChar
        jmp print_fruit

        next_itt:
        inc eax
    Loop WallCheck

    playerCheck:
    ;check collision with player
    mov dl,fruitX
    mov dh,fruitY

    cmp dl,xPos
    jne continue
    cmp dh,yPos
    jne continue

    add score,5
    jmp resetFruit

    continue:

    mov dl,fruitX
    mov dh,fruitY
    mov eax,0
    mov ecx,1000
    L1:
        cmp xPosArr[eax],dl
        jne next_itteration
        cmp yPosArr[eax],dh
        je print_coin

        next_itteration:
        inc eax
    Loop L1

    jmp no_coin

    print_coin:
    mov dl,fruitX
    mov dh,fruitY
    call gotoxy
    mov eax, yellow
    call settextcolor
    mov al,"."
    call WriteChar

    jmp print_fruit

    no_coin:
    mov dl,fruitX
    mov dh,fruitY
    call gotoxy
    mov al," "
    call WriteChar

    print_fruit:
    inc fruitY
    cmp fruitY,29
    je resetFruit

    ;again check collision with player
    mov dl,fruitX
    mov dh,fruitY

    cmp dl,xPos
    jne continue_2
    cmp dh,yPos
    jne continue_2

    add score,5
    jmp resetFruit

    continue_2:
    mov eax,cyan
    call settextcolor

    mov dl,fruitX
    mov dh,fruitY
    call gotoxy
    mov al,"@"
    call WriteChar


    jmp exitFruit

    resetFruit:
    mov fruitTimer,0
    mov fruitY,3

    random_again:
    mov eax,115
    call RandomRange
    add eax,2
    cmp al,61
    je random_again
    cmp al,78
    je random_again

    mov fruitX,al

    exitFruit:
    mov dl,0
    mov dh,0
    call gotoxy

    ret
DrawFruit ENDP

DrawLevelLogo PROC
    mov eax, SND_ASYNC
    invoke PlaySound, addr file , 0 ,eax

    mov eax,cyan
    call settextcolor

    mov dl,35
    mov dh,11
    call gotoxy
    mov edx, offset lvl_1
    call writestring

    mov dl,35
    mov dh,12
    call gotoxy
    mov edx, offset lvl_2
    call writestring

    mov dl,35
    mov dh,13
    call gotoxy
    mov edx, offset lvl_3
    call writestring

    mov dl,35
    mov dh,14
    call gotoxy
    mov edx, offset lvl_4
    call writestring

    mov dl,35
    mov dh,15
    call gotoxy
    mov edx, offset lvl_5
    call writestring

    mov dl,35
    mov dh,16
    call gotoxy
    mov edx, offset lvl_6
    call writestring


    cmp playerLevel,1
    je LevelOne
    cmp playerLevel,2
    je LevelTwo
    cmp playerLevel,3
    je LevelThree


    LevelOne:
    mov eax,brown
    call settextcolor

    mov xlvl,75
    mov ylvl,11

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset one_1
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset one_2
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset one_3
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset one_4
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset one_5
    call writestring
    inc ylvl

    jmp exitDraw
    LevelTwo:

    mov eax,brown
    call settextcolor

    mov xlvl,75
    mov ylvl,11

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset two_1
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset two_2
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset two_3
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset two_4
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset two_5
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset two_6
    call writestring
    inc ylvl

    jmp exitDraw
    LevelThree:

    mov eax,brown
    call settextcolor

    mov xlvl,75
    mov ylvl,11

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset three_1
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset three_2
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset three_3
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset three_4
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset three_5
    call writestring
    inc ylvl

    mov dl,xlvl
    mov dh,ylvl
    call gotoxy
    mov edx, offset three_6
    call writestring
    inc ylvl

    exitDraw:
    mov eax,1500
    call Delay
    call CleanScreen
    
    ret    
DrawLevelLogo ENDP

Teleportation PROC
    
    cmp playerLevel,3
    jne exitTele
    
    cmp xPos,3
    jne next_check
    cmp yPos,26
    jne next_check

    call UpdatePlayer
    mov xPos,116
    mov yPos,4
    call DrawPlayer
    jmp exitTele

    next_check:
    cmp xPos,116
    jne exitTele
    cmp yPos,4
    jne exitTele

    call UpdatePlayer
    mov xPos,3
    mov yPos,26
    call DrawPlayer

    exitTele:
    ret
Teleportation ENDP

GameOver PROC
    
    cmp lives,0
    je game_over
    cmp playerLevel,4
    je the_end
    jmp exitOver

    game_over:
    call CleanScreen
    mov eax,red
    call settextcolor

    mov xGO,10
    mov yGO,5

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset go_1
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset go_2
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset go_3
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset go_4
    call writestring

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset go_5
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset go_6
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset go_7
    call writestring
    inc yGO

 

    jmp getChoice
    the_end:
    call CleanScreen
    mov eax,green
    call settextcolor

    mov xGO,23
    mov yGO,5

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset te_1
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset te_2
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset te_3
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset te_4
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset te_5
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset te_6
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset te_7
    call writestring
    inc yGO

    getChoice:
    mov eax,green
    call settextcolor

    mov dl,53
    mov dh,15
    call gotoxy
    mov edx, offset playerName
    call writestring
    mov edx, offset scorePrompt
    call writestring
    mov eax,0
    mov ax,score
    call writedec

    mov eax,brown
    call settextcolor

    mov xGO,15
    mov yGO,20

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset quit_1
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset quit_2
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset quit_3
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset quit_4
    call writestring
    inc yGO

    ;Restart

    mov xGO,87
    mov yGO,20

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset pa_1
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset pa_2
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset pa_3
    call writestring
    inc yGO

    mov dl,xGO
    mov dh,yGO
    call gotoxy
    mov edx, offset pa_4
    call writestring
    inc yGO

    mov eax,0
    call ReadChar
    mov gameEndChoice,al

    cmp al,"r"
    jne exitOver
    mov playerLevel,1
    mov score,0
    mov lives,3
    mov eax,0
    mov ecx,3000
    resetMap:
        mov MazeX[eax],0
        mov MazeY[eax],0
        mov MazeC[eax],0

        inc eax
    Loop resetMap
    exitOver:

    ret
GameOver ENDP

ExtraLife PROC
    
    mov dl,Xlife
    mov dh,Ylife

    cmp xPos,dl
    jne no_collision
    cmp yPos,dh
    jne no_collision

    cmp lives,3
    je resetLife

    inc lives
    jmp resetLife

    no_collision:
    inc lifeStart
    cmp lifeStart,100
    jb exitLife

    inc lifeEnd
    cmp lifeEnd,1
    je againRange
    cmp lifeEnd,50
    je resetLife
    cmp lifeEnd,1
    ja DrawLife

    againRange:
    mov eax,115
    call RandomRange
    mov Xlife,al
    add Xlife,2

    mov eax,23
    call RandomRange
    mov Ylife,al
    add Ylife,3

    mov dl,Xlife
    mov dh,Ylife

    mov eax,0
    mov ecx,3000
    L1:
        cmp MazeX[eax],dl
        jne next_itt
        cmp MazeY[eax],dh
        jne next_itt

        jmp againRange

        next_itt:
        inc eax
    Loop L1

    mov eax,0
    mov ecx,1000
    L2:
        cmp xPosArr[eax],dl
        jne next_itt_1
        cmp yPosArr[eax],dh
        jne next_itt_1

        jmp againRange

        next_itt_1:
        inc eax
    Loop L2

    DrawLife:
        mov eax,lightmagenta
        call settextcolor

        mov dl,Xlife
        mov dh,Ylife
        call gotoxy
        mov eax,"H"
        call WriteChar

    jmp exitLife

    resetLife:
        mov dl,Xlife
        mov dh,Ylife
        call gotoxy
        mov eax," "
        call WriteChar

        mov Xlife,0
        mov Ylife,0
        mov lifeStart,0
        mov lifeEnd,0

    exitLife:
    ret
ExtraLife ENDP

InvisibleChar PROC
    cmp playerLevel,3
    jne exitInv

    mov dl,Xinv
    mov dh,Yinv

    cmp xPos,dl
    jne no_collision
    cmp yPos,dh
    jne no_collision

    mov pInv,100
    jmp resetInv

    no_collision:
    inc invStart
    cmp invStart,100
    jb exitInv

    inc invEnd
    cmp invEnd,1
    je againRange
    cmp invEnd,50
    je resetInv
    cmp invEnd,1
    ja DrawInv

    againRange:
    mov eax,115
    call RandomRange
    mov Xinv,al
    add Xinv,2

    mov eax,23
    call RandomRange
    mov Yinv,al
    add Yinv,3

    mov dl,Xinv
    mov dh,Yinv

    mov eax,0
    mov ecx,3000
    L1:
        cmp MazeX[eax],dl
        jne next_itt
        cmp MazeY[eax],dh
        jne next_itt

        jmp againRange

        next_itt:
        inc eax
    Loop L1

    mov eax,0
    mov ecx,1000
    L2:
        cmp xPosArr[eax],dl
        jne next_itt_1
        cmp yPosArr[eax],dh
        jne next_itt_1

        jmp againRange

        next_itt_1:
        inc eax
    Loop L2

    DrawInv:
        mov eax,lightcyan
        call settextcolor

        mov dl,Xinv
        mov dh,Yinv
        call gotoxy
        mov eax,"I"
        call WriteChar

    jmp exitInv

    resetInv:
        mov dl,Xinv
        mov dh,Yinv
        call gotoxy
        mov eax," "
        call WriteChar

        mov Xinv,0
        mov Yinv,0
        mov invStart,0
        mov invEnd,0

    exitInv:
    ret
InvisibleChar ENDP


END main