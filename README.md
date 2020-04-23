# Redirect printf() to USB Virtual COM Port on STM32H7 MCU
Enable printf() function to work with USB Virtual COM Port (STM32H743ZI)

__STM32CubeMX__
1. Turn on USB_OTG_FS with mode Device_Only
2. In the Middleware turn on USB_DEVICE with class Communication Device Class (Virtual Com Port)
3. Default settings
4. Generate Code

__main.c__
1. ```#include <stdio.h>```
2. Add code below between _/* USER CODE BEGIN 4 */_
```
int _write(int file, char *ptr, int len) { 
    CDC_Transmit_FS((uint8_t*) ptr, len); return len; 
}
```
That's it! 
Now you can use printf() anywere you want.

__NOTICE!__
When I started to using FreeRTOS I've noticed that printf displays an incorrect value.
Digging a bit, I found out that if you are enable the delay after CDC_Transmit_FS(), then everything works fine, so:
```
int _write(int file, char *ptr, int len) { 
    CDC_Transmit_FS((uint8_t*) ptr, len); 
    osDelay(1);
    return len; 
}
```
work's for me!
