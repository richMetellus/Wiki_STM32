.. _STM32ToolchainMakefileProject:

#########################################################################
STM32 toolchain (CubmeMX,STM32Programmer, GCC, Make and OpenOCD)
#########################################################################


*********
Steps
*********

Download and Install the Toolchain for Your OS Platform
===========================================================


1. Download `STM32CubeMX`_
#. Download the `STM32CubeProgrammer`_
#. Download the latest `GNU arm Embedded Toolchain <https://developer.arm.com/Tools%20and%20Software/GNU%20Toolchain>`_
#. Download Make
    
    * Windows:
        
        * `Make for Windows <https://gnuwin32.sourceforge.net/packages/make.htm>`_ binaries and
        * its `dependencies <https://gnuwin32.sourceforge.net/downlinks/make-dep-zip.php>`_

#. Download OpenOCD 
    
    * Windows:
        
        * `OpenOCd for Windows <https://gnutoolchains.com/arm-eabi/openocd/>`_

.. _STM32ToolchainsDownloadandInstalls:

Install The Downloaded Toolchains 
===================================

1. Create a folder where to keep your toolchains. 

    * I extracted the make-bin and its dependencies under ``C:\devtools\make-3.81``
    * the GNU arm toochains under ``C:\devtools\arm-gnu-toolchain-12.2.mpacbti-rel1-mingw-w64-i686-arm-none-eabi``

#. Add the toolchains path under windows environment variable

    #. on Windows search bar, search for ``env`` and click on ``Edit the system environment variable``
    #. Click on ``Environment Variables``
    #. Create a new special variable for the GCC path.

        #. on the environment varialbe windows, Click on ``New``
        #. For variable name called it ``ARMGCC_DIR`` and for value, copy the path of
           the arm gcc bin folder. ``C:\devtools\arm-gnu-toolchain-12.2.mpacbti-rel1-mingw-w64-i686-arm-none-eabi\bin``
        #. Click ``Ok``
    
    #. Add the special variable to your ``Path`` variable, and the other bin folder

        #. Select ``path``, then click on ``edit``
        #. click on ``New``
        #. Then add ``%ARMGCC_DIR%``
        #. Copy the bin folder path of ``make`` and ``openocd`` and add it ``Path`` variable
        #. click ``Ok``
    
    #. Click ``Ok``

#. Verify you have added the executable correctly.

    #. Open a command prompt terminal and type ``make -v``

        .. code-block:: c 

            C:\Users\ricky>make -v
            GNU Make 3.81
            Copyright (C) 2006  Free Software Foundation, Inc.
            This is free software; see the source for copying conditions.
            There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
            PARTICULAR PURPOSE.

            This program built for i386-pc-mingw32

            C:\Users\ricky>openocd --version
            Open On-Chip Debugger 0.12.0 (2023-06-21) [https://github.com/sysprogs/openocd]
            Licensed under GNU GPL v2
            libusb1 09e75e98b4d9ea7909e8837b7a3f00dda4589dc3
            For bug reports, read
                    http://openocd.org/doc/doxygen/bugs.html

            C:\Users\ricky>arm-none-eabi-gcc --version
            arm-none-eabi-gcc (Arm GNU Toolchain 12.2.MPACBTI-Rel1 (Build arm-12-mpacbti.34)) 12.2.1 20230214
            Copyright (C) 2022 Free Software Foundation, Inc.
            This is free software; see the source for copying conditions.  There is NO
            warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.


            C:\Users\ricky>


Start an Project for Your board
=================================

1. Open CubeMX
#. Under New Project, click on ``Access to Board Selector`` if you have a ST board.
#. Search for your board.

    * You can filter your board using the ``Board Filters`` window.

        * Under Type, I checked ``Nucleo-144``
        * under MCU/MPU series, I checked ``STM32H7``

#. Double click on the particular board you have.

    * one mine, ``Nucleo-h723zg``

#. Click ``Yes`` on Initialize all peripherals with their default Mode

    * clock configuration stays default for mine, the HSE is selected as the PLL source Mux
      
      .. image:: ../../_images/STM32LAB-1_CubeMX_ClockConfiguration.png

#. Under the ``Project Manager`` tab,

    #. Pick your ``Project Location``
    #. Under Toolchain/IDE selection menu, select ``Makefile``

#. Then click on ``Generate Code`` on top right corner.

    .. collapse:: Project structure generated
        
        .. literalinclude:: ../../_resources/Blinky-Makefile_GeneratedFiles.lst


Build the Project
====================

#. Open a terminal such as windows command prompt
#. Navigate to the location where you keep the project.
#. then run ``make``

    * The output will be a build folder containing a ``.bin`` and ``.elf`` file

            
Flash the Project
==================

#. Create a new phone make target call ``flash`` and add the following

    .. code-block:: make

        flash: all
	        openocd -f interface/stlink.cfg -f target/stm32h7x.cfg -c "program $(BUILD_DIR)/$(TARGET).elf verify reset exit"

    
    * we direct openocd to verify, then reset to run the program, then exit

    .. note:: 
       
       The location of the list of interface
       
       * <path-to-openocd-executable-dir>\share\openocd\scripts\interface

       The location of all the target

       * <path-to-openocd-executable-dir>\share\openocd\scripts\target

       I also find it not to be able to flash on command prompt unless I open stm32cubeide and
       change my toolchain to MCU ARM GCC



#. In order for ``make clean`` to work on windows since the ``rm`` command is not
   available, you need to add the git provided subcommand to the ``path`` environment variable
    
    * The path on mine where all the user command are was: ``C:\Program Files\Git\usr\bin``
      
