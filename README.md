# samsung-riscv
<html lang="en">
<body>
<h2>Basic Details</h2>
<b>Name:</b> Abhay Surya Shankar
<br>
<b>College:</b> Dayananda Sagar College Of Engineering
<br>
<b>Email:</b> <a href="mailto:abhaysurya2912@gmail.com">abhaysurya2912@gmail.com</a>
<br>
<b>GitHub Profile:</b> <a href="https://github.com/Abhay291204">Abhay291204</a>
<hr>
<!-- Task 1 -->
<details>
<p><summary>
<b>Task 1:</b> Task is to install RISC-V toolchain using VDI link provided,Compiling the C code 
</summary></p>
<b>1. Compiling C code</b>
<br><br>
<pre><code>
cd
gedit num.c
gcc num.c
./a.out</code></pre>
<br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%201/cprog_ex1.jpg"  alt=C code>
<br><br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%201/cprog_output.jpg"      alt=commands for c compilation>
<br><br>
<b>2. Object Dump and O1, Ofast Output</b>
<br><br>
<pre><code>
cat num.c
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o num.o num.c
ls -ltr num.o
</code></pre>
<br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%201/ass_cmd.jpg"    alt=Commands >
<br><br>
<pre><code>riscv64-unknown-elf-objdump -d num.o |less</code></pre>
<br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%201/obj_dump.jpg" alt=Object dump>
<br>
<br>
<b>For O1: The number of instructions were 15</b><br><br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%201/O1_ass.jpg" alt=O1 output>
<br><br>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o num.o num.c</code></pre>
<br>
<b>For Ofast: the number of instructions were 12</b>
<br><br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%201/fast_ass.jpg"  alt=Ofast output>
<br><br>
</details>
<hr>
<!--End of Task 1-->
<!-- Task 2 -->
<!-- Spike for Sum1ton -->				
<details>
<p><summary>
<b>Task 2:</b> Running the SPIKE Simulation and observe the performance under the -O1 and -Ofast Compiler optimization flags.
</summary></p>
<details>
<p><summary>1. Sum of Integers from 1 to n</summary></p>
<b>Debugging summ.o for O1</b>
<pre><code>riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o summ.o summ.c
ls -ltr summ.o
spike pk summ.o
spike -d pk summ.o</code></pre>
<b>O1 assembly output</b>
<pre>0000000000010184 &lt;main&gt;:
   10184:       ff010113                addi    sp,sp,-16
   10188:       00113423                sd      ra,8(sp)
   1018c:       04600793                li      a5,70
   10190:       fff7879b                addiw   a5,a5,-1
   10194:       fe079ee3                bnez    a5,10190 &lt;main+0xc&gt;
   10198:       00001637                lui     a2,0x1
   1019c:       96f60613                addi    a2,a2,-1681 # 96f &lt;register_fini-0xf741&gt;
   101a0:       04500593                li      a1,69
   101a4:       00021537                lui     a0,0x21
   101a8:       19050513                addi    a0,a0,400 # 21190 &lt;__clzdi2+0x48&gt;
   101ac:       26c000ef                jal     ra,10418 &lt;printf&gt;
   101b0:       00000513                li      a0,0
   101b4:       00813083                ld      ra,8(sp)
   101b8:       01010113                addi    sp,sp,16
   101bc:       00008067                ret
</pre>
<p>15 instructions for O1</p>
<br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%202/O1_spike_sum.png" alt=debugging O1>
<br><br>
<b>Debugging summ.o for Ofast</b>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o summ.o summ.c
spike pk summ.o
spike -d pk summ.o</code></pre>
<b>Ofast assembly output</b>
<pre>00000000000100b0 &lt;main&gt;:
   100b0:       00001637                lui     a2,0x1
   100b4:       00021537                lui     a0,0x21
   100b8:       ff010113                addi    sp,sp,-16
   100bc:       96f60613                addi    a2,a2,-1681 # 96f &lt;main-0xf741&gt;
   100c0:       04500593                li      a1,69
   100c4:       18050513                addi    a0,a0,384 # 21180 &lt;__clzdi2+0x44&gt;
   100c8:       00113423                sd      ra,8(sp)
   100cc:       340000ef                jal     ra,1040c &lt;printf&gt;
   100d0:       00813083                ld      ra,8(sp)
   100d4:       00000513                li      a0,0
   100d8:       01010113                addi    sp,sp,16
   100dc:       00008067                ret
</pre>
<p>12 instructions for Ofast</p>
<br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%202/Ofast_spike_sum.png" alt=debugging Ofast>
</details>	   
<!-- Spike for fibonacci -->	   
<details>
<p><summary>2. Fibonacci Sequence Generator</summary></p>
<b>Compiling Fibonacci C program</b>
<pre><code>gedit fibo.c
gcc fibo.c
./a.out</code></pre>
<pre>#include<stdio.h>
int main() {
    int n=500;
    int fib=0;
    int a=0,b=1;
    for(int i=0;fib&lt;n;i++){
       printf("%d\n",fib);
        a=b;
        b=fib;
        fib=a+b;
         }
    return 0;
}
</pre>
        <br><br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%202/fibo_output.png", alt=Fibonacci Compilation>
<br><br>
<b>Debugging fibo.o for O1</b>
<pre><code>riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o fibo.o fibo.c
spike pk fibo.o
spike -d pk fibo.o</code></pre>
<b>O1 assembly output</b>
<pre>10184:       fd010113                addi    sp,sp,-48
   10188:       02113423                sd      ra,40(sp)
   1018c:       02813023                sd      s0,32(sp)
   10190:       00913c23                sd      s1,24(sp)
   10194:       01213823                sd      s2,16(sp)
   10198:       01313423                sd      s3,8(sp)
   1019c:       00100493                li      s1,1
   101a0:       00000413                li      s0,0
   101a4:       000219b7                lui     s3,0x21
   101a8:       1f300913                li      s2,499
   101ac:       0080006f                j       101b4 &lt;main+0x30&gt;
   101b0:       00078413                mv      s0,a5
   101b4:       00040593                mv      a1,s0
   101b8:       1b098513                addi    a0,s3,432 # 211b0 &lt;__clzdi2+0x3c&gt;
   101bc:       288000ef                jal     ra,10444 <printf>
   101c0:       009407bb                addw    a5,s0,s1
   101c4:       00040493                mv      s1,s0
   101c8:       fef954e3                bge     s2,a5,101b0 &lt;main+0x2c&gt;
   101cc:       00000513                li      a0,0
   101d0:       02813083                ld      ra,40(sp)
   101d4:       02013403                ld      s0,32(sp)
   101d8:       01813483                ld      s1,24(sp)
   101dc:       01013903                ld      s2,16(sp)
   101e0:       00813983                ld      s3,8(sp)
   101e4:       03010113                addi    sp,sp,48
   101e8:       00008067                ret
</pre>
<p>26 instructions for O1</p>
<br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%202/O1_spike_fibo.png",alt=Debug O1>
<br><br>
<b>Debugging fibo.o for Ofast</b>
<pre><code>riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o fibo.o fibo.c
spike pk fibo.o
spike -d pk fibo.o</code></pre>
<b>Ofast assembly output</b>  
<pre>00000000000100b0 &lt;main&gt;:
   100b0:       fd010113                addi    sp,sp,-48
   100b4:       02813023                sd      s0,32(sp)
   100b8:       00913c23                sd      s1,24(sp)
   100bc:       01213823                sd      s2,16(sp)
   100c0:       01313423                sd      s3,8(sp)
   100c4:       02113423                sd      ra,40(sp)
   100c8:       00100493                li      s1,1
   100cc:       00000413                li      s0,0
   100d0:       000219b7                lui     s3,0x21
   100d4:       1f300913                li      s2,499
   100d8:       0080006f                j       100e0 &lt;main+0x30&gt;
   100dc:       00078413                mv      s0,a5
   100e0:       00040593                mv      a1,s0
   100e4:       1b098513                addi    a0,s3,432 # 211b0 &lt;__clzdi2+0x3c&gt;
   100e8:       35c000ef                jal     ra,10444 &lt;printf&gt;
   100ec:       009407bb                addw    a5,s0,s1
   100f0:       00040493                mv      s1,s0
   100f4:       fef954e3                bge     s2,a5,100dc &lt;main+0x2c&gt;
   100f8:       02813083                ld      ra,40(sp)
   100fc:       02013403                ld      s0,32(sp)
   10100:       01813483                ld      s1,24(sp)
   10104:       01013903                ld      s2,16(sp)
   10108:       00813983                ld      s3,8(sp)
   1010c:       00000513                li      a0,0
   10110:       03010113                addi    sp,sp,48
   10114:       00008067                ret
</pre>
<p>26 instructions for Ofast</p>
<br>
<img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%202/Ofast_spike_fibo.png",alt=Ofast debug>
<br><br>
</details>
</details>
<hr>   
<!--End of Task 2-->
<!-- Task 3 -->   
<details>
	<summary>
		<b>Task 3:</b> Identify instruction type of all the instructions of the Object dump with its exact 32 bits instruction code in the desired instruction type format.
	</summary><br>
<details>
	<p><summary>
		RISC-V Instruction Formats
	</summary></p>
<!-- Explaination -->
	
<h2>Instruction Types and Fields</h2>

<p> The RISC-V instructions are categorized into types based on their filed organization.Each type has specific fields like opcode,funct3,funct4,immediate values and register numbers. The types include:</p>

<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; R-Type:</b> Register Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; I-Type:</b> Immediate Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; S-Type:</b> Store Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; B-Type:</b> Branch Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; U-Type:</b> Upper Immediate Type <br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; J-Type:</b> Jump Type <br>

<!-- R-Type -->

<h3>RISCV R-Type Instructions</h3>

<p>R-type instructions are used for operations that involve only registers. These instructions typically perform arithmetic, logical, and shift operations.</p>

<b>Format:</b><br>

<pre>
+----------------------------------------------------------------------------------------------------------------------------------+
  funct7[31:25](7-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------------------------------+
</pre>

<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct7:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2:</b> Second source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1:</b> First source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd:</b> Destination register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>

<!-- I-Type -->

<h3>RISCV I-Type Instructions</h3>

<p>I-Type instructions cover various operations, including immediate arithmetic, load operations, and certain control flow instructions.</p>

<b>Format:</b><br>

<pre>
+----------------------------------------------------------------------------------------------------------+
  imm[31:20](12-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------+
</pre>

<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Immediate Value.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1:</b> First source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd:</b> Destination register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>

<!-- S-Type -->

<h3>RISCV S-Type Instructions</h3>

<p>S-type instructions are essential for accessing and manipulating data in memory.Used to store data from a register to memory.</p>

<b>Format:</b><br>

<pre>
+--------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31:25](11:5)(7-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | imm[11:7](4:0)(5-bits) | opcode[6:0](7-bits)
+--------------------------------------------------------------------------------------------------------------------------------------------+
</pre>

<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Immediate Value( split into imm[11:5] and imm[4:0]).<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2:</b> Second source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1:</b> First source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>

<!-- B-Type -->
      
<h3>RISCV B-Type Instructions</h3>

<p>B-type instructions are crucial for implementing control flow in programs, enabling conditional execution of code blocks.Used for conditional branches, which alter the program flow based on a comparison of register values.</p>

<b>Format:</b><br>

<pre>
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31](12)(1-bit) | imm[30:25](10:5)(6-bits) | rs2[24:20](5-bits) | rs1[19:15](5-bits) | funct3[14:12](3-bits) | imm[11:8](4:1)(4-bits) | imm[7](11)(1-bit) | opcode[6:0](7-bits)
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
</pre>

<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Immediate Value( split into imm[12], imm[10:5], imm[4:1] and imm[11]).<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2:</b> Second source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1:</b> First source register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3:</b> Further specifies the operation.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>

<!-- U-Type -->

<h3>RISCV U-Type Instructions</h3>

<p>U-Type instructions are used for operations like loading upper immediate (LUI) and adding upper immediate to PC (AUIPC).</p>

<b>Format:</b><br>

<pre>
+----------------------------------------------------------------------------------------------------------+
                  imm[31:12](20-bits)                |    rd[11:7](5-bits)      |     opcode[6:0](7-bits)
+----------------------------------------------------------------------------------------------------------+
</pre>

<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Upper 20 bits of the immediate value.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd:</b> Destination register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>

<!-- J-Type -->
      
<h3>RISCV J-Type Instructions</h3>

<p>J-type instructions in RISC-V are primarily used for unconditional jumps to specific target addresses within the program.They play a crucial role in controlling the flow of execution by transferring control to a different part of the code.</p>

<b>Format:</b><br>

<pre>
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  imm[31](20)(1-bit) | imm[30:21](10:1)(10-bits) | imm[20](11)(1-bit) | imm[19:12](19:12)(8-bits) | rd[11:7](5-bits) | opcode[6:0](7-bits)
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
</pre>

<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm:</b> Immediate Value( split into imm[20], imm[10:1], imm[11] and imm[19:12]).<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd:</b> Destination register.<br>
<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode:</b> Specifies the operation.<br>
</details>
<!-- Machine Codes -->
<details>
    <p><summary>
        Machine Codes for Different Instructions
    </summary></p>
<h3>Machine Codes:</h3>

<pre>10184:       fd010113                addi    sp,sp,-48
   10188:       02113423                sd      ra,40(sp)
   1018c:       02813023                sd      s0,32(sp)
   10190:       00913c23                sd      s1,24(sp)
   10194:       01213823                sd      s2,16(sp)
   10198:       01313423                sd      s3,8(sp)
   1019c:       00100493                li      s1,1
   101a0:       00000413                li      s0,0
   101a4:       000219b7                lui     s3,0x21
   101a8:       1f300913                li      s2,499
   101ac:       0080006f                j       101b4 &lt;main+0x30&gt;
   101b0:       00078413                mv      s0,a5
   101b4:       00040593                mv      a1,s0
   101b8:       1b098513                addi    a0,s3,432 # 211b0 &lt;__clzdi2+0x3c&gt;
   101bc:       288000ef                jal     ra,10444 &lt;printf&gt;
   101c0:       009407bb                addw    a5,s0,s1
   101c4:       00040493                mv      s1,s0
   101c8:       fef954e3                bge     s2,a5,101b0 &lt;main+0x2c&gt;
   101cc:       00000513                li      a0,0
   101d0:       02813083                ld      ra,40(sp)
   101d4:       02013403                ld      s0,32(sp)
   101d8:       01813483                ld      s1,24(sp)
   101dc:       01013903                ld      s2,16(sp)
   101e0:       00813983                ld      s3,8(sp)
   101e4:       03010113                addi    sp,sp,48
   101e8:       00008067                ret
</pre>

<!-- 1 -->

<h3>1. Machine code for <code>addi sp, sp, -48</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>addi sp, sp, -48</code>  <br><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>-48 (12 bits,two's complement) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>sp (x2,5 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>sp (x2,5 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(-48): </b><code>111111010000</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(sp=x2): </b><code>00010</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>10184:       fd010113          addi sp, sp, -48</code></pre>
  
<table>
<tr>
   <th>Immediate (12 bits)</th>
   <th>rs1 (5 bits)</th>
   <th>funct3 (3 bits)</th>
   <th>rd (5 bits)</th>
   <th>Opcode (7 bits)</th>
</tr>
<tr>
   <td>111111010000</td>
   <td>00010</td>
   <td>000</td>
   <td>00010</td>
   <td>0010011</td>
</tr>
</table>

<!-- 2 -->

<h3>2. Machine code for <code>sd ra, 40(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd ra, 40(sp)</code>  <br><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0100011 (7 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>40 (12 bits split into imm[11:5] and imm[4:0]) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Base Register(rs1): </b>sp (x2,5 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rd): </b>ra (x1,5 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(40): </b><code>000000101000 </code>(Split into imm[11:5]=<code>0000001</code> and            imm[4:0]=<code>01000</code>)<br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2(ra=x1): </b><code>00001</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0100011</code> <br><br>
<b>&nbsp;&nbsp;Binary Representation:</b><br><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[11:5] (7 bits): </b><code>0000000</code><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2 (5 bits): </b><code>00001</code><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1 (5 bits): </b><code>00010</code><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3 (3 bits): </b><code>011</code><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[4:0] (5 bits): </b><code>01000</code><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode (7 bits): </b><code>0100011</code><br><br>
<pre><code>10188:       00113c23       sd   ra, 24(sp)</code></pre>
  
<table>
<tr>
   <th>Imm[11:5] (7 bits)</th>
   <th>rs2 (5 bits)</th>
   <th>rs1 (5 bits)</th>
   <th>funct3 (3 bits)</th>
   <th>imm[4:0] (5 bits)</th>
   <th>Opcode (7 bits)</th>
</tr>
<tr>
   <td>0000000</td>
   <td>00001</td>
   <td>00010</td>
   <td>011</td>
   <td>01000</td>
   <td>0100011</td>
</tr>
</table>

<!-- 3 -->

<h3>3. Machine code for <code>sd s0, 32(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd s0, 32(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0100011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>32 (12 bits split into imm[11:5] and imm[4:0]) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Base Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rd): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(32): </b><code>000000100000 </code>(Split into imm[11:5]=<code>0000001</code> and 		imm[4:0]=<code>00000</code>)<br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0100011</code> <br><br>
 <b>&nbsp;&nbsp;Binary Representation:</b><br><br>
 	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[11:5] (7 bits): </b><code>0000001</code><br>
  	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2 (5 bits): </b><code>01000</code><br>
   	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1 (5 bits): </b><code>00010</code><br>
    	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3 (3 bits): </b><code>011</code><br>
     	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[4:0] (5 bits): </b><code>00000</code><br>
      	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode (7 bits): </b><code>0100011</code><br><br>
<pre><code>1018c:       02813023          sd   s0, 32(sp)</code></pre>
	   
<table>
	<tr>
		<th>Imm[11:5] (7 bits)</th>
		<th>rs2 (5 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>imm[4:0] (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>0000001</td>
		<td>01000</td>
		<td>00010</td>
		<td>011</td>
		<td>00000</td>
		<td>0100011</td>
	</tr>
</table>

<!-- 4 -->

<h3>4. Machine code for <code>sd s3, 8(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>sd s3, 8(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0100011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>8 (12 bits split into imm[11:5] and imm[4:0]) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Base Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rd): </b>s3 (x19,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(8): </b><code>000000001000 </code>(Split into imm[11:5]=<code>0000000</code> and 		imm[4:0]=<code>01000</code>)<br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2(s3=x19): </b><code>01001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0100011</code> <br><br>
 <b>&nbsp;&nbsp;Binary Representation:</b><br><br>
 	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[11:5] (7 bits): </b><code>0000000</code><br>
  	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs2 (5 bits): </b><code>01001</code><br>
   	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1 (5 bits): </b><code>00010</code><br>
    	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3 (3 bits): </b><code>011</code><br>
     	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; imm[4:0] (5 bits): </b><code>01000</code><br>
      	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; opcode (7 bits): </b><code>0100011</code><br><br>
<pre><code>10198:       01313423          sd   s3, 8(sp)</code></pre>
	   
<table>
	<tr>
		<th>Imm[11:5] (7 bits)</th>
		<th>rs2 (5 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>imm[4:0] (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>0000000</td>
		<td>01001</td>
		<td>00010</td>
		<td>011</td>
		<td>01000</td>
		<td>0100011</td>
	</tr>
</table>

<!-- 5 -->

<h3>5. Machine code for <code>li s1, 1</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li s1, 1</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>1 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s1 (x9,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(1): </b><code>000000000001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s1=x9): </b><code>01001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>1019c:       00100493     li   s1, 1</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000001</td>
		<td>00000</td>
		<td>000</td>
		<td>01001</td>
		<td>0010011</td>
	</tr>
</table>

<!-- 6 -->

<h3>6. Machine code for <code>li s0, 0</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li s0, 0</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(0): </b><code>000000000000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>101a0:       00000413          li   s0, 0</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>00000</td>
		<td>000</td>
		<td>01000</td>
		<td>0010011</td>
	</tr>
</table>

<!-- 7 -->

<h3>7. Machine code for <code>li a0, 0</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li a0, 0</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>a0 (x10,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(0): </b><code>000000000000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(a0=x10): </b><code>01010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>101cc:       00000513          li   a0, 0</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>00000</td>
		<td>000</td>
		<td>01010</td>
		<td>0010011</td>
	</tr>
</table>

<!-- 8 -->

<h3>8. Machine code for <code>li s2, 499</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>li s2, 499</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>499 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s2 (x18,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(0): </b><code>000111110011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s2=x18): </b><code>10010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>101a8:       1f300913          li   s2, 499</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000111110011</td>
		<td>00000</td>
		<td>000</td>
		<td>10010</td>
		<td>0010011</td>
	</tr>
</table>

<!-- 9 -->

<h3>9. Machine code for <code>mv s0, a5</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>mv s0, a5</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>a5 (x15,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(0): </b><code>000000000000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(a5=x15): </b><code>01111</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>101b0:       00078413          mv   s0, a5</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>01111</td>
		<td>000</td>
		<td>01000</td>
		<td>0010011</td>
	</tr>
</table>

<!-- 10 -->

<h3>10. Machine code for <code>mv a1, s0</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>mv a1, s0</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>s0 (x8,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>a1 (x11,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(0): </b><code>000000000000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(s0=x8): </b><code>01000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(a1=x11): </b><code>01011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>101b4:       00040593          mv   a1, s0</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>01000</td>
		<td>000</td>
		<td>01011</td>
		<td>0010011</td>
	</tr>
</table>

<!-- 11 -->

<h3>11. Machine code for <code>addi sp, sp, 48</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>addi sp, sp, 48</code>  <br><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0010011 (7 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>48 (12 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>sp (x2,5 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>sp (x2,5 bits) <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(48): </b><code>000000110000</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(sp=x2): </b><code>00010</code> <br>
    <b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0010011</code> <br><br>
<pre><code>101e4:       03010113         addi sp, sp, 48</code></pre>
  
<table>
<tr>
   <th>Immediate (12 bits)</th>
   <th>rs1 (5 bits)</th>
   <th>funct3 (3 bits)</th>
   <th>rd (5 bits)</th>
   <th>Opcode (7 bits)</th>
</tr>
<tr>
   <td>000000110000</td>
   <td>00010</td>
   <td>000</td>
   <td>00010</td>
   <td>0010011</td>
</tr>
</table>


<!-- 12 -->

<h3>12. Machine code for <code>lui s3, 0x21</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>lui s3, 0x21</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0110111 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0x21(33) (20 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s3 (x19,5 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(0x21): </b><code>00000000000000100001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s3=x19): </b><code>10011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0110111</code> <br><br>
<pre><code>101a4:       000219b7          lui  s3, 0x21</code></pre>
	   
<table>
	<tr>
		<th>Immediate (20 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>00000000000000100001</td>
		<td>10011</td>
		<td>0110111</td>
	</tr>
</table>

<!-- 13 -->

<h3>13. Machine code for <code>ld ra, 40(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ld ra, 40(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0000011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>40 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>ra (x1,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(40): </b><code>000000101000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(ra=x1): </b><code>00001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0000011</code> <br><br>
<pre><code>101d0:       02813083          ld   ra, 40(sp)</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000101000</td>
		<td>00010</td>
		<td>011</td>
		<td>00001</td>
		<td>0000011</td>
	</tr>
</table>

<!-- 14 -->

<h3>14. Machine code for <code>ld s2, 16(sp)</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ld s2, 16(sp)</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>0000011 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>16 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>sp (x2,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>s2 (x18,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>011 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(16): </b><code>000000010000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(sp=x2): </b><code>00010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(s0=x8): </b><code>10010</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>0000011</code> <br><br>
<pre><code>101dc:       01013903          ld   s2, 16(sp)</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000010000</td>
		<td>00010</td>
		<td>011</td>
		<td>10010</td>
		<td>0000011</td>
	</tr>
</table>

<!-- 15 -->

<h3>15. Machine code for <code>ret</code></h3>
<b>&nbsp;&nbsp;Instruction: </b><code>ret</code>  <br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b>1100111 (7 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate: </b>0 (12 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Source Register(rs1): </b>ra (x1,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Destination Register(rd): </b>zero (x0,5 bits) <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Function(funct3): </b>000 (3 bits) <br><br>
<b>&nbsp;&nbsp;Breakdown:</b><br><br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Immediate(1): </b><code>000000001011</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rs1(ra=x1): </b><code>00001</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; funct3: </b><code>000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; rd(zero=x0): </b><code>00000</code> <br>
	<b>&nbsp;&nbsp;&nbsp;&nbsp;&#183; Opcode: </b><code>1100111</code> <br><br>
<pre><code>101dc:       00008067     ret</code></pre>
	   
<table>
	<tr>
		<th>Immediate (12 bits)</th>
		<th>rs1 (5 bits)</th>
		<th>funct3 (3 bits)</th>
		<th>rd (5 bits)</th>
		<th>Opcode (7 bits)</th>
	</tr>
	<tr>
		<td>000000000000</td>
		<td>00001</td>
		<td>000</td>
		<td>00000</td>
		<td>1100111</td>
	</tr>
</table>
</details>
</details>
<hr>
<!--End of Task 3-->

</body>
</html>
<!-- Task 4 -->
<details><summary><b>Task 4: </b>By using RISC-V Core: Verilog netlist and Testbench, perform an experiment of Functional Simulation using GTKWave and Observe the waveforms.</summary>
    <h3>Steps:</h3>
    1. Using suitable commands install the iverilog and GTKWave in ubuntu<br>
    2. Compile the RISC-V Core: Verilog netlist and Testbench<br>
    3. Observe the waveform output in GTKWave window<br>
    <h4>Installing iverilog and GTKWave in Ubuntu:</h4>
    <pre><code>sudo apt install iverilog gtkwave</code></pre>
    <h3>Simulate and run the verilog code</h3>
    <pre><code>iverilog -o iiitb_rv32i iiitb_rv32i.v iiitb_rv32i_tb.v
    ./iiitb_rv32i
    gtkwave iiitb_rv32i.vcd</code></pre>
    <h4>GTKWave Window:</h4><br>
    <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/GTKwave.png" alt="GTKWave Window">
    <br><br>
    <h4>Hardcoded Instructions:</h4><br>
    <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/Instructions.png" alt="Hardcoded ISA">
    <br>
    <h3>Ouput Waveforms:</h3>
    <p>The output waveforms showing the instructions performed in a 5-stage pipelined architecture</p>
    <b><i>Instruction 1:</i></b><pre> ADD R6, R2, R1</pre>
        <p>This instruction Adds values of registers R2 and R1 and stores the result in register R6, In this case 1 + 2 = 3.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/ADD%20R6%20R2%20R1.png" alt="ADD R6, R2, R1">
    <br><br><b><i>Instruction 2:</i></b><pre> SUB R7, R1, R2</pre>
        <p>This instruction subtracts value of register R2 from R1 and stores the result in register R7, In this case 1 - 2 = -1.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/sub%20r7%20r1%20r2.png" alt="SUB R7, R1, R2">
    <br><br><b><i>Instruction 3:</i></b><pre> AND R8, R1, R3</pre>
        <p>This instruction executes bitwise "AND" between values of registers R1 and R3 and stores the result in register R8, In this case 01 & 11 = 01(1 in decimal).</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/AND%20R8%20R1%20R3.png" alt="AND R8, R1, R3">
    <br><br><b><i>Instruction 4:</i></b><pre> OR R9, R2, R5</pre>
        <p>This instruction executes bitwise "OR" between values of registers R2 and R5 and stores the result in register R9, In this case 010 | 101 = 111(7 in decimal).</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/or%20r9%2Cr2%2Cr5.png" alt="OR R9, R2, R5">
    <br><br><b><i>Instruction 5:</i></b><pre> XOR R10, R1, R4</pre>
        <p>This instruction executes bitwise XOR between values of registers R1 and R4 and stores the result in register R10, In this case 001 ^ 100 = 101(5 in decimal).</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/XOR%20R10%20R1%20R4.png" alt="XOR R10, R1, R4">
    <br><br><b><i>Instruction 6:</i></b><pre> SLT R11, R2, R4</pre>
        <p>This instruction checks the values of registers R2 and R4 if value of R2 is less than value of R4, then register R11 is set to 1, In this case 2<4 so R11 is set to 1.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/slt%20r11%20r2%20r4.png" alt="SLT R11, R2, R4">
    <br><br><b><i>Instruction 7:</i></b><pre> ADDI R12, R4, 5</pre>
        <p>This instruction adds the immediate data 5 to the value in register R4 and stores the result in register R12, In this case 4 + 5 = 9.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/addi%20r12%20r4%205.png" alt="ADDI R12, R4, 5">
    <br><br><b><i>Instruction 8:</i></b><pre> SW R3, R1, 2</pre>
        <p>This instruction stores the register data @R1+2 into the memory, In this case 1 + 2 = 3.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/sw%20r3%2Cr1%2C2.png" alt="SW R3, R1, 2">
    <br><br><b><i>Instruction 9:</i></b><pre> LW R13, R1, 2</pre>
        <p>This instruction loads the register data @R1+2 into the register R13, In this case 1 + 2 = 3.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/LW%20R13%20R1%202.png" alt="LW R13, R1, 2">
    <br><br><b><i>Instruction 10:</i></b><pre> BEQ R0, R0, 15</pre>
        <p>This instruction Branches to 15 instructions ahead of current instruction if values of registers R0 equals R0, so Program Counter will be incremented by 15, In this case PC is 10 so new PC value will be 10+15=25.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/beq%20r0%20r0%2015.png" alt="BEQ R0, R0, 15">
    <br><br><b><i>Instruction 11:</i></b><pre> ADD R14, R2 R2</pre>
        <p> This instruction Adds values of registers R2 and R2 and stores the result in register R14, In this case 2 + 2 = 4.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/add%20r14%2Cr2%2Cr2.png" alt="ADD R14, R2 R2">
    <br><br><b><i>Instruction 12:</i></b><pre> BNE R0, R1, 20</pre>
        <p>This instruction Branches to 20 instructions ahead of current instruction if values of registers R0 and R1 don't match , so Program Counter will be incremented by 20, In this case PC is 28 so new PC value will be 28+20=48.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/BNE%20R0%20R1%2020.png" alt="BNE R0, R1, 20">
    <br><br><b><i>Instruction 13:</i></b><pre> ADDI R12, R4, 5</pre>
        <p>This instruction adds the immediate data 5 to the value in register R4 and stores the result in register R12, In this case 4 + 5 = 9.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/addi%20r12%20r4%205.png" alt="ADDI R12, R4, 5">
    <br><br><b><i>Instruction 14:</i></b><pre> SLL R15, R1, R2</pre>
        <p>This instruction shifts the value of register R1 to left by 2, (001)&lt;&lt;2=(100)4.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/sll%20r15%20r1%20r2.png" alt="SLL R15, R1, R2">
    <br><br><b><i>Instruction 15:</i></b><pre> SRL R16, R4, R2</pre>
        <p>This instruction shifts the value of register R1 to right by 2, (100)&gt;&gt;2=(001)1.</p>
        <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%204/srl%20r16%2Cr14%2Cr2(2).png" alt="SRL R16, R4, R2">
    <br><br>
    </details>
    <!--End of Task 4-->
<hr>
<!--task 5-->
    <details>
  <summary>
      <b>
        Task 5:
      </b>
      To implement any digital circuit using VSDSquadron Mini and check whether building and uploading of C program file on RISCV processor works.  
  </summary>
  <h2>
    Implement Pseudo Noise Sequence generator using VSDSquadronmini
  </h2>

  <h3>
    Overview
  </h3>

  <p>
    This project involves the implementation of a Pseudo-Random Number (PN) Sequence Generator using the VSD Squadron Mini, a RISC-V-based SoC development kit. A PN sequence generator is a crucial component in digital communication systems, cryptography, and spread-spectrum applications. This project demonstrates the practical application of digital logic and RISC-V architecture by generating a PN sequence using Linear Feedback Shift Register (LFSR) logic.

The implementation includes reading and writing binary data through GPIO pins, designing the LFSR-based PN sequence generator, simulating the design using the PlatformIO IDE, and displaying the generated PN sequence using LEDs. This project provides a hands-on understanding of digital signal manipulation, randomness in digital systems, and the use of RISC-V for custom hardware acceleration or cryptographic applications.
  </p>

  <h3>
   Components Required
  </h3>

  <ul>
    <li>
      VSD Squadron Mini
    </li>
    <li>
      Push buttons for clock
    </li>
    <li>
      8 LEDs for Output
    </li>
    <li>
      Bread Board
    </li>
    <li>
      Jumper wires
    </li>
    <li>
     VS Code for software Development
    </li>
    <li>
    PlatformIO multi framework professional IDE
    </li>
  </ul>
  <h3>
  Hardware Connections
  </h3>
  <ul>
    <li>
      <b>
        Input:
      </b>
      <p>
        One input connected to the GPIO Pins of VSDsquadron Mini via push button mounted on the breadboard.
      </p>
    </li>
    <li>
      <b>
        Output:
      </b>
      <p>
       Eight LEDs are connected to display the result of PN Sequence.
      </p>
    </li>
    <li>
      <p>
      The GPIO pins are configured according to the reference mannual ensuring the correct flow of signals between the components.
      </p>
    </li>
  </ul>
  <br>
  <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%205/Circuit%20diagram.png">
  <br>

  <h3>
Block Diagram
  </h3>
  <img src="https://github.com/Abhay291204/samsung-riscv/blob/main/Task%205/The-circuit-diagram-and-symbol-of-LFSR-8-bit.png">
  <br>
  <p>
This block diagram represents an 8-bit Linear Feedback Shift Register (LFSR) using D flip-flops and XOR gates.  
  </p>
  <ul>
<li>
The top section shows the LFSR as a shift register with feedback from specific bits XORed together.
</li>

<li>
The bottom section is the implementation using 8 D flip-flops (DFF-0 to DFF-7) connected in series, where each flip-flop stores one bit.
</li>

<li>
The XOR gate takes inputs from selected flip-flop outputs and feeds the result back to the first flip-flop, generating a pseudo-random sequence.
</li>

<li>
The clock signal synchronizes the shifting operation.
</li>
  </ul>
  <h3>
    Working
  </h3>
  <p>
    The most common type of PN sequence generator is based on a Linear Feedback Shift Register (LFSR). Here's how it works:
  </p>

  <p>
<b>Shift Register:</b> A shift register is a series of memory cells (flip-flops) that can store binary data. The data in the register is shifted one position to the right with each clock pulse.

  </p>


  <p>
  <b>  Feedback:</b> The output of certain stages (taps) of the shift register are combined using an XOR (exclusive OR) gate. The output of the XOR gate is fed back into the first stage of the shift register.
  </p>


  <p>
  <b>  Sequence Generation:</b> With each clock pulse, the data in the shift register is shifted, and the XOR feedback is inserted into the first stage. The output of the last stage of the shift register is taken as the output of the PN sequence generator.
  </p>

  <p>
    I have used a push button for providing clock pulse ,when the push button in pressed high pulse gets fed to the flip-flops and the 8bit pn sequence is generated.
  <h3>Truth Table for PN Sequence</h3>
<table>
  <!--Row 1-->
<tr>
  <th>Clk</th> <th>FF<sub>0</sub></th> <th>FF<sub>1</sub></th> <th>FF<sub>2</sub></th> 
  <th>FF<sub>3</sub></th> <th>FF<sub>4</sub></th> <th>FF<sub>5</sub></th> <th>FF<sub>6</sub></th> <th>FF<sub>7</sub></th>
</tr>

  <!--Row 2-->
<tr>
  <td>0</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td>
</tr>

  <!--Row 3-->
<tr>
  <td>1</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td>
</tr>

  <!--Row 4-->
<tr>
  <td>2</td> <td>1</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td>
</tr>

  <!--Row 5-->
<tr>
  <td>3</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td> <td>0</td>
</tr>

  <!--Row 6-->
<tr>
  <td>4</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td> <td>0</td>
</tr>
  <!--Row 7-->
<tr>
  <td>5</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>0</td> <td>0</td>
</tr>
  <!--Row 8-->
<tr>
  <td>6</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>0</td>
</tr>
  <!--Row 9-->
<tr>
  <td>7</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td>
</tr>
  <!--Row 10-->
<tr>
  <td>8</td> <td>0</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td>
</tr>
  <!--Row 11-->
<tr>
  <td>9</td> <td>1</td> <td>0</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td>
</tr>
  <!--Row 12-->
<tr>
  <td>10</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td>
</tr>

  <!--Row 13-->
<tr>
  <td>11</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>1</td> <td>1</td> <td>1</td>
</tr>
  <!--Row 14-->
<tr>
  <td>12</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>1</td> <td>1</td>
</tr>
  <!--Row 15-->
<tr>
  <td>13</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>1</td>
</tr>

  <!--Row 16-->
<tr>
  <td>14</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td>
</tr>

  <!--Row 17-->
<tr>
  <td>15</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td> <td>1</td> <td>0</td>
</tr>
</table>

<h3>
  Code
</h3>
<pre>
#include&lt;stdio.h&gt;
#include&lt;debug.h&gt;
#include&lt;ch32v00x.h&gt;
#include&lt;math.h&gt;
#define N 8  // Define N as a macro
void GPIO_Config(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0}; 
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU; // Defined as Input Type.
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2 | GPIO_Pin_3 | GPIO_Pin_5 | GPIO_Pin_6 | GPIO_Pin_7;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);
    GPIO_Init(GPIOC, &GPIO_InitStructure);
}
int main()
{
    int lsfr[N] = {1, 0, 0, 0, 0, 0, 0, 0};  // Initialize all elements
    int lsfr_1[N];
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);
    SystemCoreClockUpdate();
    Delay_Init();
    GPIO_Config();
    GPIO_WriteBit(GPIOD, GPIO_Pin_3,SET);
    while(1)
    {
        if(GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_4)==0){
        uint8_t feedback = lsfr[0] ^lsfr[N-1];
        for (int i = 0; i &lt; N; i++) {
lsfr_1[(i + 1) % N] = lsfr[i];
        }
lsfr_1[0] = feedback;
        for (int i = 0; i &lt; N; i++) {
            lsfr[i] = lsfr_1[i];
        }
        GPIO_WriteBit(GPIOC, GPIO_Pin_3, (lsfr[7]) ? SET : RESET);
        GPIO_WriteBit(GPIOC, GPIO_Pin_2, (lsfr[6]) ? SET : RESET);
        GPIO_WriteBit(GPIOC, GPIO_Pin_1, (lsfr[5]) ? SET : RESET);
        GPIO_WriteBit(GPIOC, GPIO_Pin_0, (lsfr[4]) ? SET : RESET);
        GPIO_WriteBit(GPIOC, GPIO_Pin_6, (lsfr[3]) ? SET : RESET);
        GPIO_WriteBit(GPIOC, GPIO_Pin_7, (lsfr[2]) ? SET : RESET);
        GPIO_WriteBit(GPIOD, GPIO_Pin_2, (lsfr[1]) ? SET : RESET);
        GPIO_WriteBit(GPIOD, GPIO_Pin_3, (lsfr[0]) ? SET : RESET);
        Delay_Ms(500);
        }   
    }

}
</pre>


</details>

<hr>
PN Sequence_Implementation_Video
<p>
  Higher Quality Video
</p>
<ul>
  <li>
<a href="https://youtu.be/PRGi8_yfsbM"
target="_blank">
 Youtube Link
</a>
</li>
</ul>



