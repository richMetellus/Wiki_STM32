.. _STM32blinkyExamples:

#############################################
STM32 First Program - Blinky
#############################################

***************
Objectives
***************

This is a continuation of :ref:`STM32 and Nucleo Introduction <stm32andNucleoIntroJournal>`
This is where I document different methods, IDE or no IDE of making a program
that blank the STM32 based product.

For this demo, I will simply blink an LED

*********************************
Your First Program
*********************************

Pre-req:

1. You already have STM32CubeIDE installed.
2. The ST-Link drivers are installed.
#. Internet Connection if this is your first time initiating a chip or board

Hardware:

1. Any nucleo board. I am using `NUCLEO-L476RG`

Configuring
=============

Steps:

1. Open STM32CubeIDE
2. ``File`` --> ``New`` --> ``STM32`` Project

    * From there you will be presented with a target selection window
    * From this page, you'll have the choice of choosing:

        * MCU/MPU Selector
        * Board Selector


3. Choose the target MCU or board and double click its name:
    
    * Since I am  using a nucleo-board, I select ``Board Selector``
    * In the Part number search, I look for ``nucleo-L476RG`` since that was the
      board I am using
    * double click or click next

      * This will populate the the board with the default configuration as 
        described in the newly auto-generated .ioc file.

4. Choose a name for your project. 
    
    * you can prefix the name of the project with your board: ex: nucleo-l476rg-blinky
    * Target Language: C
    * Targeted Binary Type: Executable
    * Targeted Project Type: STM32Cube

#. Click on ``Finish``

    * You'll be prompted with a ``Initialize all peripherals with their default mode``

        * Shawn Hymmel recommends, selection ``No`` if you are working with a bare
          chip and only turing on the peripherals as needed. This way you'll have better control 
          over which peripherals are on by default.
        
    
        #. Since this is a demo board, click ``Yes``

            * This will Initialize the peripherals like UART that can
              help with debugging.

    * You'll be prompted with ``Device Configuration Tool editor is associated
      with Device Configuration Tool perspective. Do you want to open this 
      perspective now?``
      
       A perspective in Eclipse is a set of windows, panes, and visuals that 
       take up the IDE in support of a particular feature or programming mode.

        #. Click ``Yes``
            
            .. note::
                Note that STM32 cube IDE does not come with board definitions 
                and individual part libraries by default, as a result, whenever 
                you create your first project for a particular chip or board, 
                the IDE will download all the associated files for that chip or board.

                So you'll need an internet connection for the first time initialization
                of the chip or board.

            * Observations:

                * I see that download and unzip of stm32cube-fw_l4_v1172 is done
                * IOC

            * By default, you should have the peripherals and pins enabled to 
              support the bare minimum of Nucleo board functionality 
              (LED, button, oscillators, and USART).
            
            .. note::
                note that the CubeMX offers a powerful, graphical way to 
                initialize peripherals and pins on your microcontroller. 
                If you click on a pin, you get a list of peripherals that pin 
                supports. If you click on one, you can enable the peripheral on 
                that pin. For example, clicking GPIO_Output will turn that pin 
                into an output (ready to toggle some digital logic). 
    
    * Explore the STM32CubeMX perspective.

        * For example, in the ``Pinout & Configuration`` tab, in the ``Pinout View`` showing the chip,
          you can click on ``PC8`` pin and set it as a ``GPIO_input``, ``GPIO_output``,
          ``GPIO_analog`` and other configuration
        
        * ``PA5`` is configured already as a GPIO_output that is connected to the
          green LED2 on the nucleo-l476rg board.
          
            .. image:: ../../_images/STM32LAB-0_PinA5_Output_LED2.png

        * Explore the ``Clock Configuration`` tab:

            .. image:: ../../_images/STM32LAB-0_ClockConfiguration.png
            
            * The clock configuration tab gives us a schematic like view showing 
              how all the clocks, dividers and multipliers are connected.

                .. important::
                    This is all set for us because we selected a nucleo board.
                    For your own board, you'll have to set them yourselves.
        
        * Explore the ``Project Manager`` tab:
          
            .. image:: ../../_images/STM32LAB-0_ProjectManagerTab.png
            
            * The project manager tab lets us change some of the project settings, 
              such as the name and default allocated memory sizes.
        
        * Explore the ``Tool Tab``

            .. image:: ../../_images/STM32LAB-0_STM32CubeMXpersepective_ToolsTab.png
            
            * The tools tab has some advanced features such as a calculator for 
              estimating the current draw of your microcontroller.
        

#. Click on ``File`` --> ``Save`` and you'll be asked to generate code.

#. Click ``Yes``

    * I was prompted for ``This file is associated with C/C++ perspectives`` do
      you want to open it and I clicked ``yes``

        * main.c is opened up
    
    .. collapse:: The List of Files created
       
       .. literalinclude:: ../../_resources/STM32/Blinky_FilesGenerated.lst
    
    * Explore some of the autogenerated files:

        * In ``main.c`` you'll see this:
          
          .. code-block:: c
             :linenos:
             :lineno-start: 64

             int main(void)
             {
               /* USER CODE BEGIN 1 */
             
               /* USER CODE END 1 */
               
          .. code-block:: c
             :linenos:
             :lineno-start: 94
             
               /* Infinite loop */
               /* USER CODE BEGIN WHILE */
               while (1)
               {
                 /* USER CODE END WHILE */
             
                 /* USER CODE BEGIN 3 */
               }
               /* USER CODE END 3 */
             }
        
          * In main.c , you 'll want to put your setup code just before the ``while`` loop

          * explore the ``static void MX_USART2_UART_Init(void)`` to see how the UART
            is initialized or see how some library functions being called to
            initialize system clocks and various peripherals.
          
          .. warning:: 
             
             say we write some some code between one of the comment guards, 
             and write some other code that exists outside a set of begin and 
             end the comment guards I'll then save my work.
             
             Next we go back into the graphical tool and change a hardware configuration,
             make some change such a switching a pin to outut and then save.

             Click save and it ask us to generate code again, click yes,
             see the code that was written between the comment guards persisted,
             however the code outside the guards was deleted.
          
          .. important::
             
             You can use assembly C, C++ ARM's CMSIS library, or the manufacturers 
             specific libraries for arm programming, but which library should you use?
             If you take a look in ``Project``--> ``Properties`` --> ``C/ C++ Build``
             -> ``Settings``, you can see the toolchain being used, the free
             GNU tools
             
             .. image:: ../../_images/STM32LAB-0_Blinky_Toolchains.png
             
             If you head to Arm's site, it has a good overview of how the 
             libraries work together.

             * ARM created the `Cortex Microcontroller Software Interface Standard 
               (CMSIS) <https://developer.arm.com/tools-and-software/embedded/cmsis>`_
               to be a set of libraries that help you control registers and set up 
               peripherals on any arm [cortex] controller.

                .. image:: ../../_images/STM32LAB-0_ARM_CMSIS.png
                
                * This is great for learning arm development but in general it
                  won't necessarily create portable code as each microcontroller
                  can have different registers and peripherals as defined by the
                  silcon manufacture.
                
                * ST's hardware abstraction layer (HAL) makes controlling some
                  of the peripherals in STM32 easier as it handle much of the
                  setup for us.

                    * **In most cases, if you write HAL code for one STM32 microcontroller,
                      it should be able to run on another STM32 microcontroller.**
                      (Code Portability)
                    
                    * To find more documentation in your particular HAL, google for
                      ``stm32xx HAL`` ex: ``stm32h7 Hal``


Write Blinky: Toggle LED
==========================

ST's hardware abstraction layer (HAL) makes controlling some
of the peripherals in STM32 easier as it handle much of the
setup for us.

* **In most cases, if you write HAL code for one STM32 microcontroller,
    it should be able t  o run on another STM32 microcontroller.**
    (Code Portability)

* To find more documentation in your particular HAL, google for ``stm32xx HAL``
* **To find information about blinking an LED, the GPIO section will be a good place
  to start.**

    * For STM32L4, the document is the UM1884 explaing the HAL and low-layer
      drivers.

A stripped down version of main.c file in the source code directory within our 
projects is as shown below.

.. code-block:: c

   #include "main.h"
    
    
   void SystemClock_Config(void);
   static void MX_GPIO_Init(void);
   static void MX_USART2_UART_Init(void);

   int main(void)
   {
    
     HAL_Init();
    
     SystemClock_Config();
    
     MX_GPIO_Init();
     MX_USART2_UART_Init();

     while (1)
     {
    
     }
    
   }
   

These functions ``SystemClock_Config()``, ``MX_GPIO_Init()``  and ``MX_USART2_UART_Init`` 
are generated by CubeMX to configure the system clock and and the GPIO pins as shown
in the Pinout selection. 
The implementation of all these functions is found in the file after the main function.

We call each of them before the main loop while(1) as well as the HAL_Init function. 
The HAL_Init must be called at the beginning of your application. Its functionality 
is clarified in the HAL Documentation as shown below.

.. code-block:: text

   3.12.2.1 HAL global initialization

            In addition to the peripheral initialization and de-initialization 
            functions, a set of APIs are provided to initialize the HAL core 
            implemented in file stm32l4xx_hal.c. 
            
            * HAL_Init(): this function must be called at application startup to 
                - initialize data/instruction cache and pre-fetch queue 
                - set SysTick timer to generate an interrupt each 1ms 
                  (based on HSI clock) with the lowest priority 
                - call HAL_MspInit() user callback function to perform system 
                  level initializations (Clock, GPIOs, DMA, interrupts). 
                  HAL_MspInit() is defined as “weak” empty function in the HAL drivers
   
And most importantly it initializes the SysTick timer, whose ticks are used by 
the ``HAL_Delay()``. The SysTick timer is set to tick @ 1000Hz or every 1mSec. 
So the HAL_Delay function will give you multiples of milliseconds delay

.. code-block:: text
   :caption: from the UM1884 manual

   * HAL_Delay(). this function implements a delay (expressed in milliseconds) using the SysTick timer. 
     Care must be taken when using HAL_Delay() since this function provides an 
     accurate delay (expressed in milliseconds) based on a variable incremented 
     in SysTick ISR. This means that if HAL_Delay() is called from a peripheral 
     ISR, then the SysTick interrupt must have highest priority (numerically lower) 
     than the peripheral interrupt, otherwise the caller ISR is blocked


Besides the delay function, we also need to know the HAL APIs for controlling 
the GPIO pins. To do basic stuff like pin read or write or port read/write, 
and so on.

From the HAL documentation on the GPIO chapters, the following APIs are available:

.. image:: ../../_images/STM32LAB-0_GPIO_IO_APIs.png

take a closer look at the GPIO_WritePin() function.::

    Function name
        void HAL_GPIO_WritePin (GPIO_TypeDef * GPIOx, uint16_t GPIO_Pin, GPIO_PinState PinState) 
    
    Function description Set or clear the selected data port bit. Parameters
        * GPIOx: where x can be (A..H) to select the GPIO peripheral for STM32L4 
          family
        * GPIO_Pin: specifies the port bit to be written. This parameter can 
          be any combination of GPIO_Pin_x where x can be (0..15).
        * PinState: specifies the value to be written to the selected bit. 
          This parameter can be one of the GPIO_PinState enum values: 
            
            - GPIO_PIN_RESET: to clear the port pin 
            - GPIO_PIN_SET: to set the port pin
    
    Return values 
        None:

    Notes
        This function uses GPIOx_BSRR and GPIOx_BRR registers to allow atomic 
        read/modify accesses. In this way, there is no risk of an IRQ occurring 
        between the read and the modify access

**Steps**

1. Inside the while(1) loop, add the following two lines for the nucleo-L476rg:
   
   Note this code is dependent on your configuration. The pinout might be different
   if you're using a different board.

    .. code-block:: c
       
       HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);
       HAL_Delay(1000);

    * This is the actual blinky code: we're telling GPIO Port A, Pin 5 (PA5) 
      to toggle every 1000 ms. If you look above the while(1) loop, you will 
      see the following function call: ``MX_GPIO_Init();``


The finish code should look like that:

.. tabs::
   
   .. tab:: main.c (blinky)
      
      TODO: include final code here

#. Save the code

Run and Debug The code
===========================

#. From the code, click ``Project`` > ``Build Project``

    * the will compile and link the appropriate libraries.

#. Connect The ST-Link To The USB Port & SWD Pins On Board
   If your board is already have a the ST-Link debugger on it, just plug it in

#. For running the code on the board: ``Run`` > ``Run as`` > ``STM32 MCU C/C++ Application``

#. For a debugging session:

   #. With the ``main.c`` program open, click 
      Run the code: click ``Run`` > ``Debug As`` > ``STM32 MCU C/C++ Application``

      * You should get a pop-up window asking you to set the debug configurations. 

          #. Leave everything as default and click OK. 

      * When asked about switching perspectives, click Switch. You should get a 
        new perspective with a new toolbar at the top of your IDE. 
      
      * once the debug section start, watch the console to see it is successful.
        The debugger should start and on my stm32 nucleo board, the st-link 
        LD1 keep blinking from red to green.

        .. collapse:: Console output

           .. code-block:: bash

              STMicroelectronics ST-LINK GDB server. Version 7.1.0
              Copyright (c) 2022, STMicroelectronics. All rights reserved.
              
              Starting server with the following options:
                      Persistent Mode            : Disabled
                      Logging Level              : 1
                      Listen Port Number         : 61234
                      Status Refresh Delay       : 15s
                      Verbose Mode               : Disabled
                      SWD Debug                  : Enabled
                      InitWhile                  : Enabled
              
              Waiting for debugger connection...
              Debugger connected
              Waiting for debugger connection...
              Debugger connected
              Waiting for debugger connection...
                    -------------------------------------------------------------------
                                     STM32CubeProgrammer v2.12.0                  
                    -------------------------------------------------------------------
              
              
              
              Log output file:   C:\Users\ricky\AppData\Local\Temp\STM32CubeProgrammer_a20828.log
              ST-LINK SN  : 066AFF485671664867214650
              ST-LINK FW  : V2J40M27
              Board       : NUCLEO-L476RG
              Voltage     : 3.25V
              SWD freq    : 4000 KHz
              Connect mode: Under Reset
              Reset mode  : Hardware reset
              Device ID   : 0x415
              Revision ID : Rev 4
              Device name : STM32L4x1/STM32L475xx/STM32L476xx/STM32L486xx
              Flash size  : 1 MBytes
              Device type : MCU
              Device CPU  : Cortex-M4
              BL Version  : 0x92
              Debug in Low Power mode enabled
              
              
              
              Memory Programming ...
              Opening and parsing file: ST-LINK_GDB_server_a20828.srec
                File          : ST-LINK_GDB_server_a20828.srec
                Size          : 11.97 KB 
                Address       : 0x08000000 
              
              
              Erasing memory corresponding to segment 0:
              Erasing internal memory sectors [0 5]
              Download in Progress:
              
              
              File download complete
              Time elapsed during download operation: 00:00:00.486
              
              
              
              Verifying ...
              
              
              
              
              Download verified successfully 
                  
      * You can set breakpoint, 
      * you can walk thorough the code by clicking the ``step into`` button
        to see the internal step, even the assembly instruction that get called
        by toggeling the ``insturctions stepping mode`` button
      * you can click ``step over`` button to skip to the next code line instead
        of exploring (step into) the current hightlight code line.

      * Click the ``Resume`` button to let the code just run without clossing the
        debugging session.

#. Test: The green LED on your Nucleo board (labeled LD2) should begin to flash 
   on for 1 second and off for 1 second.

Debugging/ Code Step-through
==============================

If you double-click on a line number (e.g. 102 a), you can add a breakpoint 
(shown by the hook-like symbol to the left of the line number). 

* The code will stop execution at this line. You can then use the Step Into, 
  Step Over, and Step Return buttons (to the right of the Stop button)

To step through lines of code, one at a time.

* Step Into: If you are currently on a function call, go into that functions 
  definition to execute lines of code one at a time. If not on a function call, 
  execute the line of code.

* Step Over: If you are currently on a function call, execute all the code 
  within that function without going into the function's definition. If not on a 
  function call, execute the line of code.

* Step Return: If you are currently inside a function definition, execute the 
  rest of the code in that function and return from the function


#. You can Click The Debug Button To Compile The Code & Flash It To The Board & 
   Start A Debugging Session
#. You Can Stop The Debugging Session Or Keep It Going. But You Need To Restart 
   The MCU Once To Run The New Application At The Booting Process.