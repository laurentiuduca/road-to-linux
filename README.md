### Road to Linux on RISC-V in Verilog
risc-v dual-core in $30 fpga board
* the book can be read for free
* if you use our work, please cite us<br>
Road to Linux on RISC-V in FPGA / Laurenţiu Cristian Duca and Anton Duca. - 2nd ed. -
Smart Publishing, 2024, ISBN 978-606-068-087-1

#### RLSoC dual core
##### Status
* easily switch between single and dual core by using a flag
* ported to the cheap Tang nano 20k board <br>
  (you must install GoWin EDA education build <br>
   and add gowin/IDE/bin folder to your system path)
* try true dual-core rlsoc-tn-20240526-1700.tgz<br>
* please see the <a href="https://github.com/laurentiuduca/rlsoc2">rlsoc2</a> repository for verilog <br>
(git tag 1.1 contents were copied to the src folder <br>
in order to have the rlsoc-tn-20240526-1700.tgz version)
* git tag 1.3 is with PLIC and a mmu translation optimization
* git tag 1.5 allows compressed instructions
* 25 pages dedicated chapter in the book, see chapter 8!
* https://youtu.be/Ce9SjgnXosQ 

##### news: if you want to use rlsoc2 git tag 1.3 and above,
* for linux in devicetree.dts, 
memory@80000000 {
		reg = <0x0 0x80000000 0x0 0x770000>;
* for verilog in define.vh `define D_START_PC      32'h80770000
* for bbl set MEM_START to 0x80770000 in the riscv-pk/configure
* and in riscv-pk/configure.ac:
AC_SUBST([MEM_START], [0x80770000], [Physical memory start address])
* recompile sw and hw system


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
* compile bbl
```
cd rlsoc-tn/riscv-pk-build
../riscv-pk/configure --enable-logo --enable-print-device-tree --host=riscv32-buildroot-linux-gnu --with-arch=rv32imac --with-payload=../linux-kernel/vmlinux
make
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
* initial RLSoC sources are rlsoc1-20220521-1310.tgz used in chapters 1-7
<!--
* for Verilator 5, in src/Makefile must be appended -Wno-LATCH to VERIFLAGS:
```
VERIFLAGS += -Wno-WIDTH -Wno-CASEINCOMPLETE -Wno-COMBDLY -Wno-LATCH
```
-->
* simulation:
```
cd src
make veri
./simv
```
* for SIM_MODE, the time to boot is greater than 80000000 clock cycles
* demo video on youtube <br>
https://www.youtube.com/watch?v=1YQXtjbE5Q8

#### Try apache nuttx on rlsoc2
from the <a href="https://github.com/laurentiuduca/nuttx-rlsoc">nuttx-rlsoc</a> repository<br>

#### Support
If you feel so, please contact me - 
laurentiu dot duca at gmail dot com

