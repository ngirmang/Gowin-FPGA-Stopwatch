# Gowin FPGA Stopwatch (TangNano20k version)

 ![image](./diagram/pic.jpg)

Verilog codes for a stopwatch project using the Tang Nano 20K. LED clock display used is the **Common Cathode** pinout. This is for the GW2AR-LV18QN88PC8/I7 FPGA developement board with TDCG1060m 7 LED Segment display.
 
# Physical Setup
Use the following suggested layout with two half breadboards stuck together:

![image](./diagram/setup.png)

Red dots on the 4 digit 7 segment display show the where the pins on the package exist for the TDCG1060M 4 digit 7 segment display.

Components:
* Tang Nano 20K FPGA board
* Two half breadboards.
* USB C cable (to connect to the programming PC)
* A 4 digit 7 segment display (TDC_1060M where _ can be either `R` for red or `G` for green)
* At least 14 wires (the suggested hook-up layout above uses 22)

For more details, consult the .cst file.
 
# Some dev notes
 * Each `always` block operates asynchronously, meaning that any codes nested within an `always` will run in parallel/simultaneously with other existing `always`. Hence this creates a runtime limitation that prevents any assignments (e.g. x = 1'b1;) of the same parameter to happen in separate `always` block during runtime. Question, which parallel/simultaneous process will have priority over the assignment?
 * When an `always` statement includes more than 1 condition, e.g. `always @ (posedge sys_clk or posedge sys_rst_n)`, said `always` block requires at least one `if` statement with either condition to be nested within the block.
 * There are two types of bitwise shift operator, `>>>` and `>>` (`<<<` and `<<` for reverse direction). Triple brackets are arithmetic shift where as double brackets are logical shifts. The `>>>` operator is unique from the other shift operator ( `<<<`, `<<` and `>>`), where by it will fill the left side of a **signed** parameter with a copy of the left most bit value, e.g. `4'b1011 >>> 1; //becomes 4'b1101 after shift`. The other shift operator simply fills the first or last bit with 0, depending on which shift direction.
 * Tang Nano 20k uses a 27 Mhz crystal oscillator, meaning its clock speed is basically 27 million pulses. Hence, a clock pulse counter that resets at 2.7 million pulses is equivalent to 0.1 seconds.
 * The project has "Use SSPI as regular IO" checkbox checked in the project settings (`Project>Configuration>Place & Route>Dual Purpose Pin>Use SSPI as regular IO`) in order to use all the pins on the `S2` side of the Tang Nano 20K that are not voltage rails or ground pins. If you download this directly for use in the Gowin IDE, I believe it should aready be set upon opening.

# Useful links for further study
 * https://www.chipverify.com/verilog/verilog-tutorial (General starting point)
 * https://www.asic-world.com/verilog/veritut.html (Intermediate and detailed guide, good for code reference)
 * https://wiki.sipeed.com/hardware/en/tang/tang-nano-20k/nano-20k.html (Tang Nano wiki)
 * https://www.vishay.com/doc/?83180= (PDF of 7 Segment LED clock display datasheet)
