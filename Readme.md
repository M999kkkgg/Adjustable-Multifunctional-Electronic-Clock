# Adjustable Multifunctional Electronic Clock
## Description
In the course of "Microcomputer Principle and interface technology", I learned that the section of 8253 programmable timing counter involves the use of 8253 to make electronic clock. At present, the design of electronic clock microcomputer in the network has great limitations and can not meet the needs of users, so I want to combine the contents learned this semester on this basis, 8259 interrupt controller is used to further expand the function of electronic clock. The fixed interrupt priority of 8259 is ir0 to ir7 from high to low, and the priority of the master chip can be set higher than that of the slave chip. The operation control word ocw1 can set to mask the interrupt of ir0 to ir7 on a chip 8259; So my idea is to use the interrupt service program of the master chip to set the interrupt mask word of ocw1 of the slave chip, so that when counting, the slave chip only accepts the interrupt from 8253 to complete the update timing, and the timing can be modified and controlled when the counting is suspended. In addition, I use two pieces of 6264 to simulate the construction of parity memory, which is used to store the hours, minutes and seconds of immediate timing. Its corresponding CPU memory address range is f0000h-f3fffh.
## Project Function
The system realizes the following functions。
> 1. Realize the cycle timing function, that is, from 00:00:00 to 23:59:59, and automatically update to 00:00:00 when the next second comes.
> 2. Achieve timing pause.
> 3. When the selection is realized during pause, the setting number of minutes and seconds is modified
> 4. Auto increment modification (auto carry) of minutes and seconds when selecting during pause
> 5. When the selection is realized during pause, the self subtraction modification of minutes and seconds (automatic lowering)
> 6. Speed up the timing
> 7. Slow down the timing
> 8. Resume timing initial speed

The above functions are interactive through the keys in the interface.
> 1. The console end is equipped with 9 buttons, 1 switch, a two bit binary input and an eight bit binary input.
> 2. The switch controls the total counting state. It needs to be off before starting. If it is off, the counting will be forcibly stopped. The functions of each button have been marked in detail in the circuit diagram.
> 3. Two binary inputs are used to control the selection of operation bits. 00 controls the second bit, 01 controls the minute bit, 10 controls the time bit, and 11 is invalid control. Here, the control second bit is not set in 00,01,10 by default in the software.
> 4. Eight digit binary is used for setting number input. In order to facilitate display, the number entered by the user is processed according to decimal system. For non decimal number, decimal adjustment is carried out in the software. For decimal number greater than 59, it is forcibly converted to 59H in the software.
## Hardware Design
Please see the document named **Report.pdf** for details.
## Software Design
The data segment is defined in the program, in which two bytes are stored. Flag is the flag bit, which is used to display the current counting status. 00h indicates that counting is currently in progress, 01h indicates that the current counting is suspended, and the default status in the program is suspended counting, i.e. 01h. CLOCK_ Speed is used to store the initial counting value of the current 8253 (which can also be understood as counting speed). The default value in the program is 0ah. Its value will be modified during speed increase, speed decrease and recovery. The allowable range of modification is 05H to 0Fh.  
Two 8255, two 8259 and one 8253 are used in this project.  
8255_ 1's address is 8000h, 8002h, 8004h, 8006h, 8255_ 2's address is 9000h, 9002h, 9004h, 9006h; 8259_ The address of 1 is a000h, a002h, 8259_ The address of 2 is b000h, b002h; The addresses of 8253 are c000h, c002h, c004h and c006h. The data of hours, minutes and seconds are stored in 6264, two bytes each (to avoid the error of accessing parity memory), the second storage address is 8000h: 0100h, the sub storage address is 8000h: 0200h, and the hour storage address is 8000h: 0300h.
Other information is detailed in the document named **Report.pdf**.
## Test
See the document named **Report.pdf** for details of test information.
## Circuit Diagram
![img](/Project/Circuit%20Diagram/Project_00.png)
