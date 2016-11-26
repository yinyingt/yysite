---
layout: post
published: true
title: Get Started with STM32VLDiscovery on Linux
mathjax: false
related: true
comments: true
---

_Last updated: Jan, 2015_

ARM-based microcontroller (e.g., ARM Cortex-Mx) development boards often offer much more goodies than the popular Arduino platforms, and better bang for the buck as well. However, the ARM development has a higher learning curve as compared with Arduino for beginners. The purpose of this tutorial is to jumpstart ARM development for beginners by showing how to get started with the popular low-cost (~$10) [STM32VLDiscovery](http://www.st.com/web/en/catalog/tools/FM116/SC959/SS1532/PF250863) board which builds around a STM32F100 core (ARM Cortex-M3). The procedure can also be applied to other ST boards with some modifications. 

This tutorial draws significant mount of aspiration from Geoffrey Brown's [Discovering the STM32 Microcontroller](http://www.cs.indiana.edu/~geobrown/book.pdf).

# Build ARM Toolchain 

The difficulty in beginning ARM development is partially a result of getting and familiarizing with a toolchain. An ARM toolchain usually refers to the combination of compiler, linker, libraries, GUI (optional but often included), programmer and debugger. There are various such toolchains developed and built by different people, costing from thousands of dollars (e.g., Keil, IAR) to free. Most of them contain IDEs such as Eclipse. IDEs can be very useful to streamline big projects. However, as opposed to many people's thoughts, I think IDEs hide details of what is behind the stage which is pedagogically useful. Here I use the old-school "bare-metal" toolchain, which doesn't have an IDE. 


Here is how I build such a toolchain on Linux . 

## Compiler, linker, etc.

[Sourcery Codebench Lite Edition](http://www.mentor.com/embedded-software/sourcery-tools/sourcery-codebench/editions/lite-edition/) is a free gcc-based "bare-metal" development suite. Note that if the Linux is 64-bit, some 32-bit libraries should be installed before the Sourcery Codebench toolchain is usable, as described [here](https://sourcery.mentor.com/GNUToolchain/kbentry62). For Ubuntu 12.04 and later, the following command works: 

{% highlight bash %}
apt-get install libgtk2.0-0:i386 libxtst6:i386 gtk2-engines-murrine:i386 \
lib32stdc++6 libxt6:i386 libdbus-glib-1-2:i386 libasound2:i386
{% endhighlight %}

Download the edition for [ARM EABI](http://www.mentor.com/embedded-software/sourcery-tools/sourcery-codebench/editions/lite-edition/request?id=e023fac2-e611-476b-a702-90eabb2aeca8&downloadlite=scblite2012&fmpath=/embedded-software/sourcery-tools/sourcery-codebench/editions/lite-edition/form) after a simple sign-up. You either choose a Linux installer (.bin file) or just a tar ball. (_Update in Jan, 2015: Unfortunately, the Source Codebench Lite Edition is no longer freely available for ARM processors. However, a previous version (late in 2013) is accessible [here](https://drive.google.com/file/d/0B-02Vsq-9f3YVWtseG1ydG9lc2s/view?usp=sharing)_)

If you get an installer, such as "arm-2013.11-24-arm-none-eabi.bin" at the time of the writing, run

{% highlight bash %}
sh arm-2013.11-24-arm-none-eabi.bin
{% endhighlight %}

and follow the GUI wizard. You can also download the tar ball, extract to a directory, and add the directory to the PATH like

{% highlight bash %}
export PATH=$PATH:xxx/bin
{% endhighlight %}

## Library

Download the [STM32F10x standard peripheral library](http://www.st.com/web/en/catalog/tools/FM147/CL1794/SC961/SS1743/PF257890) and extract to some place. This library will be used later. 

## Debugger

The STM32VL discovery board has an on-board implementation of [STLINK](http://www.st.com/web/catalog/tools/FM146/CL1984/SC720/SS1454/PF219866), interfacing the host computer with a USB link. Here I use the popular open-source [stlink](https://github.com/texane/stlink), a STLINK (both V1 and V2) programmer and debugger application for STM32 discovery boards. This application is written for Linux (and I don't have any luck of getting stlink to work under Windows anyway). After pulling down its git source

{% highlight bash %}
git clone git@github.com:texane/stlink.git
{% endhighlight %}

or directly downloading from [here](https://github.com/texane/stlink/archive/master.zip), the installation guide is well documented in its [README](https://github.com/texane/stlink/blob/master/README) file. The installation command lines can be summarized as 

{% highlight bash %}
sudo apt-get install libusb-1.0.0 libusb-1.0.0-dev pkg-config autoconf automake gcc g++
./autogen.sh
./configure
make
sudo cp stlink_v1.modprobe.conf /etc/modprobe.d  # add modprobe rules 
sudo modprobe -r usb-storage && modprobe usb-storage
sudo cp 49-stlink*.rules /etc/udev/rules.d/
sudo restart udev   # add udev rules
{% endhighlight %}


# The First Program

Blinking an on-board LED is often an equivalent version of  writing a "hello world" program in software development. And this is something I am going to show here. 

[Geoffrey Brown's C program template](https://github.com/geoffreymbrown/STM32-Template) for STM32 discovery boards offers a great starting point for such a "hello world" type program. Some of the heavy lifting has been done by "[startup_stm32f10x.c](https://github.com/geoffreymbrown/STM32-Template/blob/master/startup_stm32f10x.c)", "[stm32f100.ld](https://github.com/geoffreymbrown/STM32-Template/blob/master/stm32f100.ld)" (linker script), and "[Makefile.common](https://github.com/geoffreymbrown/STM32-Template/blob/master/Makefile.common)". The template can be used for multiple projects. The idea is that each individual project will have its own folder which includes project-specific files (e.g., project source code, makefile, etc.), and keep the common code under the root directory and let them be shared among projects. 

Writing a LED blinking program can be described in three steps as follows. 

First, modify the "[Makefile.common](https://github.com/geoffreymbrown/STM32-Template/blob/master/Makefile.common)", which is to be included by the project-specific makefiles. Usually, you just need to change "TOOLROOT" and "LIBROOT" to the paths of Sourcery Codebench binaries and STM32F10x standard peripheral library. 

Second, create a folder named "Blink_simple". And create a file "main.c" 

{% highlight C %}
# include <stm32f10x.h>
# include <stm32f10x_rcc.h>
# include <stm32f10x_gpio.h>

void Delay(__IO uint32_t nCount); 

int main(void)
{      
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_StructInit (& GPIO_InitStructure);

    /* Configure all unused GPIO port pins in Analog Input mode (floating input
      trigger OFF), this will reduce the power consumption and increase the device
      immunity against EMI/EMC *************************************************/
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_GPIOB |
    RCC_APB2Periph_GPIOC | RCC_APB2Periph_GPIOD |
    RCC_APB2Periph_GPIOE, ENABLE);
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_All;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AIN;
    GPIO_Init(GPIOA, &GPIO_InitStructure);
    GPIO_Init(GPIOB, &GPIO_InitStructure);
    GPIO_Init(GPIOC, &GPIO_InitStructure);
    GPIO_Init(GPIOD, &GPIO_InitStructure);
    GPIO_Init(GPIOE, &GPIO_InitStructure);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_GPIOB |
    RCC_APB2Periph_GPIOC | RCC_APB2Periph_GPIOD |
    RCC_APB2Periph_GPIOE, DISABLE);

    // Enable Peripheral Clocks
    RCC_APB2PeriphClockCmd (RCC_APB2Periph_GPIOC, ENABLE);

    // Configure Pins
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;   // intialize the content of GPIO_InitStructure
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;  // output push-pull 
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_2MHz;  // use the lowest speed possible 
    GPIO_Init (GPIOC, & GPIO_InitStructure);

   while (1) {
        static int ledval = 0;
        GPIO_WriteBit (GPIOC, GPIO_Pin_9, ledval ? Bit_SET:Bit_RESET); 
        ledval = 1 - ledval;
        Delay(100000);
    }    
}

void Delay(__IO uint32_t nCount) {
    for(; nCount != 0; nCount--);
}

#ifdef USE_FULL_ASSERT
void assert_failed ( uint8_t * file , uint32_t line)
{
/* Infinite loop. Use GDB to find out why we're here */
while (1);
}
#endif
{% endhighlight %}

and a "Makefile"

{% highlight bash %}
TEMPLATEROOT = ..

# additional compilation flags
CFLAGS += -O0 -g
ASFLAGS += -g

# project files
OBJS= $(STARTUP) main.o
OBJS += stm32f10x_gpio.o stm32f10x_rcc.o

# include common make file
include $(TEMPLATEROOT)/Makefile.common
{% endhighlight %}

The program will swing the Pin 9 of GPIO C between digital 1 and 0, and thus blink the on-board green LED which is connected to this pin.

To compile the program, simple type 

{% highlight bash %}
make
{% endhighlight %}

Lastly, it is time to load and debug. The way it works is stlink first establishes a gdb server listening at some port, and gdb acts as a client and interface with the hardware via stlink. In this case, debugger gdb can also program the microcontroller (burning code into the microcontroller ROM). Type 

{% highlight bash %}
st-util -1   # note the option flag is -1 (number 1) not -l (letter l)
{% endhighlight %}

which initialize a gdb server listening at port 4242 by default. The "-1" flag means STLINK version 1. The debugging session can be started by 

{% highlight bash %}
arm-none-eabi-gdb Blink_simple.elf
{% endhighlight %}

Now it enters a gdb interactive shell. Establish the connection with gdb server and load the program binary by 

{% highlight bash %}
target extended-remote :4242  # or "tar ext :4242" for short
load    # or "l" for short
{% endhighlight %}

To just run the program, type 

{% highlight bash %}
run
{% endhighlight %}

To set breakpoints and start debugging, type 

{% highlight bash %}
break 37   # set a breakpoint at Line 37
continue    # or "c" for short
{% endhighlight %}

To list the breakpoints and delete breakpoints, type 

{% highlight bash %}
info break
delete 1  # delete the first listed breakpoint
delete   # delete all breakpoints
{% endhighlight %}

A "Ctrl+c" can interrupt the debugging, and a "Ctrl+d" can exit the gdb session.  More gdb commands are available online as a part of gdb tutorials. 

This should be the first program!
