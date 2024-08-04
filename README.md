# Interfacing with an Encoder on Nucleo 411RE 

Encoder implemented on an STM32 Nucleo board 411RE or 446ZE

One of the goals is to accuractely keep counts over several revolutions. I configured the TIM2 Encoder option to a count period of 2000 for the encoder. This means the TIM2-CNT register will count 0-2000 for a complete rotation and then return to 0 when moving in the clockwise direction. It will count 2000 to 0 when moving in anti-clockwise. 

A quick way of dealing with the zero crossing is to assume that I am not going to see a change of 1500 counts in 10ms. That would be 150,000 in a second or 75 rotations in 1 second. Much slower than the 3000 rpm the motor is rated for. I would increase the TIM2 count period  and the sampling rate for higher speeds.

# encoder index 

For future implementation.
I will try to use this to reset aligne the zero crossing with the index.

# serial output 

![image](https://github.com/user-attachments/assets/8fc3ecfb-f9ba-4ec1-b0bb-b7cc6a14fb65)

![image](https://github.com/user-attachments/assets/5af03fa4-ee3c-44c4-8b53-91da02621608)

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
![image](https://github.com/user-attachments/assets/8e9af533-268f-41f8-a2b5-930f4519b74e)

ENC_A is mapped to PA0 (TIM1) with pull-down enabled 
ENC_B is mapped to PA1 (TIM2) with pull-down enabled 

# Add Z Index 

![image](https://github.com/user-attachments/assets/36da41dc-9108-47ef-8c01-dadd8a28efa3)

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

![TEK0013](https://github.com/user-attachments/assets/76938b3b-378d-4225-a845-8d27d645a501)

![TEK0015](https://github.com/user-attachments/assets/0563076c-17d1-421c-bfcf-8f172acff74a)


# References 

https://deepbluembedded.com/stm32-timer-encoder-mode-stm32-rotary-encoder-interfacing/

Application Note: AN4776
