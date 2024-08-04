# Interfacing with an Encoder on Nucleo 411RE 

Encoder implemented on an STM32 Nucleo board 411RE or 446ZE

The coder is 500 counts. At Quadrature it will be 2000 counts. That means entering 1999 into the TIM2:Counter Period register. 

I will capture the offset on the falling endge of the ENC Z input which I have hooked to an ExtI interrupt. We should see this number twice in a rotation in 1000 count increments. These numbers should be consistent. We should also be able to accuration keep track over several rotations in both the clockwise and anti-clockwise directions.  

# Track several rotations

The TIM2 Counter will return, inclusive 0 up to 1999 or, inclusive 1999 downto 0 (2000 counts total).

A quick way of dealing with the zero crossing is to assume that I am not going to see a change of 1500 counts in 10ms. That would be 150,000 in a second or 75 rotations in 1 second. Much slower than the 3000 rpm the motor is rated for. I would increase the TIM2 count period  and the sampling rate for higher speeds.

# Serial output 

## Clockwise Rotation 

![image](https://github.com/user-attachments/assets/e9839733-3261-4e3c-821a-4c5240cb2db6)

## Anti-Clockwise Rotation 

![image](https://github.com/user-attachments/assets/5193f816-149e-49d7-818c-26be79a9be0c)


# FreeRTOS

I am using ST middleware implementation of FreeRTOS 10.3.1  

Honestly, it's not doing much aside from providing a nice thread to run code in. If I add features I will use more tasks and queues to safefly move data around. 

# Patterns 

  Clock-Wise:      AB -> 00  01  11  10

  Ant-Clock Wise:  AB -> 00  10  11  01 

# the failed attempt 

Tried to do this manually with Rising/Falling triggers on A and B. I found I was missing most of the state changes for B. I will try this again with A and B on different ExitI See if I can get them into the xQueue in the correct order for processing. 

# Encoder Option on Timer Input 

## TIM2 Settings

![image](https://github.com/user-attachments/assets/a241932e-1b66-4eea-8b13-7e5b053bddf3)

ENC_A is mapped to PA0 (TIM1) with pull-down enabled 
ENC_B is mapped to PA1 (TIM2) with pull-down enabled 

# Add Z Index 

![image](https://github.com/user-attachments/assets/a0cf20b4-8ad5-4417-8f1a-3092a851cbfd)

# Encoder wiring 

red on orange - 5V
red on pink - A
black on orange - gnd
black on pink - B
red on yellow - Z (index)

red on greay - Hall A
black on yellow - Hall B
block on gray - Hall C

# From the Scope 

CH1 Yellow: Enc A
CH3 Magenta: Enc B
CH2 Blue: Enc Z (Index)

![TEK0014](https://github.com/user-attachments/assets/b268fd6a-1852-40fa-8ac7-b7fb2fe4e708)

![TEK0015](https://github.com/user-attachments/assets/0563076c-17d1-421c-bfcf-8f172acff74a)


# References 

https://deepbluembedded.com/stm32-timer-encoder-mode-stm32-rotary-encoder-interfacing/

Application Note: AN4776
