## from the silicon to the web

> what you need is a terminal and text editor to lock in and cook

![20240808_221354](https://github.com/user-attachments/assets/03bdde88-62d3-499a-a971-e44fc9bcbf82)

Hiring is hard, a lot of modern CS education is really bad, and it's hard to find people who understand the modern computer stack from first principles.

This project is about fun and learning. It is about the journey, NOT THE DESTINATION. Enjoy yourself. If you aren't, take a break and try something entirely different.

Perhaps this project is what you've been thinking about. Moving past academic exercises, this is computing and why does it suck so bad?

Let's proceed thoughtfully and make informed decisions. Can we code in languages that are less prone to bugs? i.e., avoid Python and C, think OCaml, Haskell, and Rust or even ZIG!!. Things worth learning, not trash you'll rapidily code followed by days of debugging. We're crafting the entire stack, we have choices.

What's changed is there's tons of compute available for compilation. Self-hosting is explicitly not a goal. Compilers should engage more in "search" and "model" processes, rather than mere "translation".

> ***building fully on software if you ever get 12 free weeks again***

#### Section 1: Intro: Cheating our way past the transistor -- 0.5 weeks
- So about those transistors -- Course overview. Describe how FPGAs are buildable using transistors, and that ICs are just collections of transistors in a nice reliable package. Understand the LUTs and stuff. Talk briefly about the theory of transistors, but all projects must build on each other so we can’t build one.
- Emulation -- Building on real hardware limits the reach of this approach. Using something like Verilator will allow anyone with a computer to play.

#### Section 2: Bringup: What language is hardware coded in? -- 0.5 weeks
- Blinking an LED(Verilog) -- Your first little program! Getting the simulator working. Learning Verilog.
- Building a UART(Verilog) -- An intro section to Verilog, copy a real UART, introducing the concept of MMIO, though the serial port may be semihosting. Serial test echo program and led control.

#### Section 3: Processor: What is a processor anyway? -- 3 weeks
- Coding an assembler(Python) -- Straightforward and boring, write in python. Happens in parallel with the CPU building. Teaches you ARM assembly. Initially outputs just binary files, but changed when you write a linker.
- Building a ARM7 CPU(Verilog) -- Break this into subsections. A simple pipeline to start, decode, fetch, execute. How much BRAM do we have? We need at least 1MB, DDR would be hard I think, maybe an SRAM. Simulatable and synthesizable.
- Coding a bootrom(Assembler) -- This allows code download into RAM over the serial port, and is baked into the FPGA image. Cute test programs run on this.

#### Section 4: Compiler: A “high” level language -- 3 weeks
- Building a C compiler(Haskell) -- A bit more interesting, cover the basics of compiler design. Write in haskell. Write a parser. Break this into subchapters. Outputs ARM assembly.
- Building a linker(Python) -- If you are clever, this should take a day. Output elf files. Use for testing with QEMU, semihosting.
- libc + malloc(C) -- The gateway to more complicated programs. libc is only half here, things like memcpy and memset and printf, but no syscall wrappers.
- Building an ethernet controller(Verilog) -- Talk to a real PHY, consider carefully MMIO design.
- Writing a bootloader(C) -- Write ethernet program to boot kernel over UDP. First thing written in C. Maybe don’t redownload over serial each time and embed in FPGA image.

#### Section 5: Operating System: Software we take for granted -- 3 weeks
- Building an MMU(Verilog) -- ARM9ish, explain TLBs and other fun things. Maybe also a memory controller, depending on how the FPGA is, then add the init code to your bootloader.
- Building an operating system(C) -- UNIXish, only user space threads. (open, read, write, close), (fork, execve, wait, sleep, exit), (mmap, munmap, mprotect). Consider the debug interface you are using, ranging from printf to perhaps a gdbremote stub into kernel. Break into subsections.
- Talking to an SD card(Verilog) -- The last hardware you have to do. And a driver
- FAT(C) -- A real filesystem, I think fat is the simplest
- init, shell, download, cat, ls, rm(C) -- Your first user space programs.

#### Section 6: Browser: Coming online -- 1 week
- Building a TCP stack(C) -- Probably coded in the kernel, integrate the ethernet driver into the kernel. Add support for networking syscalls to kernel. (send, recv, bind, connect)
- telnetd, the power of being multiprocess(C) --  Written in C, user can connect multiple times with telnet. Really just a bind shell.
- Space saving dynamic linking(C) -- Because we can, explain how dynamic linker is just a user space program. Changes to linker required.
- So about that web(C) -- A “nice” text based web browser, using ANSI and terminal niceness. Dynamically linked and nice, nice as you want.

#### Section 7: Physical: Running on real hardware -- 1 week
- Talking to an FPGA(C) -- A little code for the USB MCU to bitbang JTAG.
- Building an FPGA board -- Board design, FPGA BGA reflow, FPGA flash, a 50mhz clock, a USB JTAG port and flasher(no special hardware, a little cypress usb mcu to do jtag), a few leds, a reset button, a serial port(USB-FTDI) also powering via USB, an sd card, expansion connector(ide cable?), and an ethernet port. Optional, expansion board, host USB port, NTSC TV out, an ISA port, and PS/2 connector on the board to taunt you. We provide a toaster oven and a multimeter thermometer to do reflow. 
- Bringup -- Compiling and downloading the Verilog for the board


### tidbit

This project draws inspiration and is deeply influenced from several prominent figures in computer science and technology:

These individuals have pushed the boundaries of what's possible in computing, often by reimagining fundamental concepts and building from the ground up.

- **George Hotz** - tinygrad | comma ai. [Personal Website](https://geohot.com/)
  
- **Fabrice Bellard** - QEMU, FFmpeg. [Personal Website](http://bellard.org/)
  
- **John Carmack** - VR [oculus]. [Twitter](https://twitter.com/ID_AA_Carmack)
  
- **Linus Torvalds** - Linux | Git. [GitHub](https://github.com/torvalds)
  
- **Andrej Karpathy** - prev. dir. tesla | openai [Twitter](https://twitter.com/karpathy)

We hold deep respect for these trailblazers, whose innovative thinking and contributions have redefined the landscape of technology. Their work continues to inspire and guide our own endeavors.
