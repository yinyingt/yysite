---
layout: post
title: Quick Reference of STM32
mathjax: false
related: false
comments: true
published: true
---


_Created: Aug, 2015_

_Last modified: Dec, 2015_


## STM32F4Discovery Board

### "Unknown target connected" error when downloading program via ST-LINK/V2 in Keil

Solution: It turns out ST-LINK doesn't support JTAG programming. Go to "Options for target" --> "Debug" tab --> "Settings" to the right of "Use ST-Link Debugger" --> "Debug" tab --> under "Port" dropdown menu select "SW" instead of "JTAG". 

Ref: [this link](http://www.amobbs.com/thread-5508482-1-1.html)


### How to use the good old STM32F4 Standard Peripheral Libraries in Keil 5

Although the latest default STM32F4 library in Keil is STM32CubeF4, one can roll back to the old version as follows: 

* Launch Keil pack installer, and install "Keil:STM32F4xx_DFP" with version 1.0.8 (the last version before STM32CubeF4). 

* When selecting the software pack, select "Keil::STM32F4xx_DFP" 1.0.8, and select "fix" in the drop-down menu. 


### The pin map of the 6-pin SWD headers on Discovery boards


* Pin 1 (the pin with a white dot nearby): PWR

* Pin 2: SWCLK

* Pin 3: GND

* Pin 4: SWDIO

* Pin 5: NRST

* Pin 6: SWO


### How to output master clock on a GPIO pin

This can be useful for debugging purpose. See [this post](https://my.st.com/public/STe2ecommunities/mcu/Lists/cortex_mx_stm32/Flat.aspx?RootFolder=https%3a%2f%2fmy%2est%2ecom%2fpublic%2fSTe2ecommunities%2fmcu%2fLists%2fcortex_mx_stm32%2fMaster%20Clock%20Out%20%28MCO%29&FolderCTID=0x01200200770978C69A1141439FE559EB459D7580009C4E14902C3CDE46A77F0FFD06506F5B&currentviews=585) on STM32 forum. Check the information in the reference manual of specific STM32 MCUs. The "MCO" output pin can be found in corresponding datasheet. For example, on STM32F030, it can be done with the code as follows (actually according to the reference manual MCO prescaler is not available for STM32F030 MCUs in hardware level):

```
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8; 
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF; 
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; 
	GPIO_Init(GPIOA,&GPIO_InitStructure); 
	RCC_MCOConfig(RCC_MCOSource_SYSCLK, RCC_MCOPrescaler_1); 
```

The above code uses the old standard peripheral libraries.

## Links

* [A good summary on the register-level clock configuration on STM32F4](http://embeddedturkey.org/stm32f4xx-clock-configuration/)

