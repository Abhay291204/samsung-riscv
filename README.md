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
</body>
</html>
