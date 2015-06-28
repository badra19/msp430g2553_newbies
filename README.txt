I.  Introduction

The master code receives the measurement data from the designated slave via I2C. The Master converts the I2C data to ASCII characters and sends it to the Main CPU across UART which is running PUTTY.  

Build each *.c file as the main source file in 2 different projects in CodeComposer Studio. I used CodeComposer Studio v.5.05.00.

    http://processors.wiki.ti.com/index.php/Download_CCS 

(Note that you will need to register with TI to download.)

See schematic of the circuit in feed_back_system.PNG.
   
We are using the TI MSP430 Launchpad:
    1. To implement I2C, remove P1.^ jumper to LED2
    2. To implement UART, place TX/RX jumpers in parallel with board writing. 

II. Code Description

I2C is a serial communication protocol designed to send bytes (an 8-bit number) of data, one bit at a time. This process is also known as a “bit-bang”.  I2C requires 2 lines: a data line (SDA) which both sends and receives bits and a clock line (SCL) to count off each data bit.  The master’s clock controls the timing.  In our system slaves are capable of sending multiple bytes. 
 
Since there may be multiple slaves (up to 128), each slave must have its own unique address. For our code, the slave addresses are in hex: 0x48, 0x4A, 0x4C … etc. Note that the slave addresses are even i.e. the least significant bit is 0 in binary. An even address designates the slave to write data according to I2C protocol. 

For the most part, we modified existing example code provided by TI.  While Launchpad and Code Composer Studio are meant to work out of box, we found the UART signal would conflict with the USB communication. Besides interfering with the debugging program, it also interfered with our mouse. If the mouse is jumping all over the screen, it is because UART is interfering with USB communication.

We solved this conflict with the aid of a USB to UART Bridge, CP2102. Essentially, this chip splits off the UART communication from the rest of the USB communication.  To use this bridge, it is necessary to install the appropriate drivers. Find the CP2102 driver here:

    http://www.silabs.com/products/mcu/pages/usbtouartbridgevcpdrivers.aspx 

With CP2102, we had to implement 2 computers: one to program, which we will	call the "programming” computer and one to serve as the Main CPU.  We were able to program and debug the microcontrollers. When the feedback system is	free-running the “programming” computer and its USB connection will be removed. 

PuTTy allows the user to handle UART communication.  With PuTTY running, the user may enter and/or read out the characters between the "main" CPU and the master MSP430.  Find the (compressed) PuTTY file here:

	http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

The user interacts with Master program by running PuTTY on the Main CPU. To properly run PuTTY, the user must designate the serial line (e.g. COMM4) and the baud rate [bps].  The baud rate is a measurement of the symbols per second, in our case it is 9600 characters per second.

UART sends ASCII characters serially (one-by-one). Each character on a keyboard has an ASCII code.  Find the ASCII coed here:

	http://www.asciitable.com/



