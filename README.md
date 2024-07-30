# stmnucleo_encoder
Encoder implemented on an STM32 Nucleo board 411RE or 446ZE

# FreeRTOS

I am using ST middleware implementation of FreeRTOS 10.3.1  

Honestly, it's not doing much aside from providing a nice thread to run code in. If I add features I will use more tasks and queues to safefly move data around. 

# Patterns 

  Clock-Wise:      AB -> 00  01  11  10

  Ant-Clock Wise:  AB -> 00  10  11  01 

# the failed attempt 

Tried to do this manually with Rising/Falling triggers on A and B. I found I was missing most of the state changes for B. I will try this again with A and B on different ExitI See if I can get them into the xQueue in the correct order for processing. 

# Encoder Option on Timer Input 



# Encoder wiring 

red on orange - 5V
red on pink - A
black on orange - gnd
black on pink - B
red on yellow - Z (index)

red on greay - Hall A
black on yellow - Hall B
block on gray - Hall C

# References 

https://deepbluembedded.com/stm32-timer-encoder-mode-stm32-rotary-encoder-interfacing/

Application Note: AN4776
