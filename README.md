# samsung-riscv
<h2>Basic Details</h2>
<b>Name:</b> Abhay Surya Shankar
<br>
<b>College:</b> Dayananda Sagar College Of Engineering
<br>
<b>Email:</b> <a href="abhaysurya2912@gmail.com">abhaysurya2912@gmail.com</a>
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

