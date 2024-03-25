### Road to Linux on RISC-V in Verilog
an accessible way of learning system bring-up
* the book can be read for free

#### RLSoC dual core
##### Status
* easily switch between single and dual core by using a flag
* ported to the cheap Tang nano 20k board <br>
  (you must install GoWin EDA education build <br>
   and add gowin/IDE/bin folder to your system path)
* rlsoc-tn-20231103.tgz<br>
please see the rlsoc2 repository for verilog <br>
(its contents must be copied to the src folder <br>
in order to try true dual-core made in 2024 which is in alpha version)
* 25 pages dedicated chapter in the book
* https://youtu.be/Ce9SjgnXosQ 

##### Implementation on tang nano 20k
* unzip bin-tn.tgz
* write linux on sd card - replace /dev/sdb with your sd drive
```
dd if=initmem.bin of=/dev/sdb bs=512 seek=0 && sync
```
* program fpga and wait until 5 leds become on
```
openFPGALoader --board tangnano20k project.fs
```
* watch linux booting on serial port at 115200
(please be aware that tangnano has 2 serial ports on the same usb wire)

##### Simulation in verilator
* first, compile bbl
```
cd rlsoc-tn/riscv-pk-build
../riscv-pk/configure --enable-logo --enable-print-device-tree --host=riscv32-buildroot-linux-gnu --with-arch=rv32imac --with-payload=../linux-kernel/vmlinux
make
```
* patch linux arch/riscv/kernel/sbi.c and add the ipi clear function
```
static const struct riscv_ipi_ops sbi_ipi_ops = {
        .ipi_inject = sbi_send_cpumask_ipi,
        .ipi_clear  = sbi_clear_ipi
};
```
* compile linux and rootfs (by using the configure files from linux-kernel folder)
and copy vmlinux to rlsoc-tn/linux-kernel
* run the simulation in verilator
```
cd rlsoc-tn/rvsoc_src_ver053/src
./run-sim.sh
```

#### Initial RLSoC
##### Notes
* initial RLSoC sources are rvsoc_src_ver053-20220521-1310.tgz used in chapters 1-7
* for Verilator 5, in src/Makefile must be appended -Wno-LATCH to VERIFLAGS:
```
VERIFLAGS += -Wno-WIDTH -Wno-CASEINCOMPLETE -Wno-COMBDLY -Wno-LATCH
```
* in src/run-sim.sh replace
```
cd /home/laur/lucru/cn/riscv/rvsoc-site-japan/initmem_gen2
with
cd ../../initmem_gen2
and
cd /home/laur/lucru/cn/riscv/rvsoc-site-japan/rvsoc_src_ver053/src
with
cd ../rvsoc_src_ver053/src
```
* for SIM_MODE, the time to boot is greater than 80000000 clock cycles
* demo video on youtube <br>
https://www.youtube.com/watch?v=1YQXtjbE5Q8


##### Book description for RLSoC
* ebook, 216 pages pdf version
(only the pseudo dual-core doc for the moment)
* RLSoC single and dual core
* The book presents the architecture of the RLSoC single and pseudo dual core project and the TinyEMU simulator.
It covers building, interactive simulation and implementation aspects. 
In the book are described the RISC-V32 processor, MMU, memory controller, console and interrupts.
We dig into the Linux kernel and see how it manages boot-up and hardware interaction.
* the book is written based on my notes taken when I tried to understand rvsoc and tinyemu 
and is based on commented code excerpts
* it is a book to be read thorough
* this version is forked from RVSoC v53, is written in Verilog 
and implements RISC-V32 with MMU, console and uses Linux initramfs disk.
* it is ported on Tang Nano 20k, Nexys A7, Arty A7 FPGA boards

##### Reader's profile
* level: intermediate to advanced
* should be skilled in assembler, C, verilog and computer architecture
* should have the ability to read pseudocode

##### Contents
  Please also see the table of contents:<br>
https://github.com/laurentiuduca/road-to-linux/blob/master/table-of-contents.pdf<br>
<img src="cover.jpg" alt="cover" width="50%"/>


##### About the author
Laurentiu-Cristian Duca is an open source software enthusiast
passioned on the Linux operating system and FPGA programming.
He started and is currently envolved in open source projects
like rtnet-preempt_rt, iot-ota-sus, openVeriFLA and other.
Laurentiu is skilled in C, verilog, assembler, Linux and computer architecture.
He has a diploma in computer science and engineering
and has got a PhD in software for electrical devices.
