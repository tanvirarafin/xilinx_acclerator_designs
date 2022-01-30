# Acclerator designs in Xilinx ZCU 104 boards

In this document, we describe how to use ZCU 104 boards for running basic acclerator design. We will use the VM created for the lab and use it for the experiments.

Also note that the VM can supoort simulation of Alveo U200 boads too.

## Creating Vector Addition Application

   - Open Vitis workspace you were using before.
   - Select **File -> New -> Application Project**.
   - Click **next**
   - Select **xilinx_zcu104_base_202120_1** as platform, click **next**.
   - Name the project **vadd**, click **next**.
   - Set Domain to **linux on psu_cortexa53**, set **Sys_root path** to ```/opt/xilinx/commons/xilinx-zynqmp-common-v2021.2/ir/sysroots/cortexa72-cortexa53-xilinx-linux```. Set the **Root FS** to ```/opt/xilinx/commons/xilinx-zynqmp-common-v2021.2/rootfs.ext4``` and **Kernel Image** to ```/opt/xilinx/commons/xilinx-zynqmp-common-v2021.2/Image```. Then click **next**.
   - Select **System Optimization Examples -> Vector Addition** and click **finish** to generate the application.
   - In the Explorer window double click the **vadd.prj** file to open it, change the **Active Build configuration** from **Emulation-SW** to **Hardware**.
   - Select **vadd_system** in Explorer window and Click **Build** icon in toolbar.

## Running Vector Addition Application on the Board

   - Write **/home/mta/workspace/vadd_system/Hardware/package/sd_card.img** into SD Card with SD Card image writer applications like Etcher on Windows or dd on Linux.
   - Put the SD card in ZCU104 board. On the board SW6 configuration switches shpuld be set as: switch 1 should be in ON condition and Switch 2-4 Should be in OFF condition.
   - Connect the serial cable with the board and the host PC.
   - Check serial port connection. 

   ```bash
	dmesg | grep tty
   ```
   
   - Above command will list serial ports e.g. tty0, ttyUSB0, ttyUSB1 etc. 
   - Run 
	
   ```bash
	sudo picocom /dev/ttyUSB1 -b 115200 -l
   ```
   
   - Turn on the board. You should see the following messages in the piccom terminal
	
   ```bash
	Xilinx Zynq MP First Stage Boot Loader 
	Release 2021.2   Oct 20 2021  -  22:38:02
	...
   ```
   
   - Using the piccom terminal, go to auto mounted FAT32 partition

   ```bash
   cd /mnt/sd-mmcblk0p1
   ```

   - Run vadd application

   ```bash
   ./vadd binary_container_1.xclbin
   ```

   - It should show program prints and XRT debug info.

   ```
   TEST PASSED
   ```
   
## Additional Information

	- Detailed set-up instructions https://github.com/Xilinx/Vitis-Tutorials/tree/2021.2/Vitis_Platform_Creation/Introduction/02-Edge-AI-ZCU104
	
	- Youtube video on how to set up Vitis for ZCU 104 using pre-built packages: https://www.youtube.com/watch?v=mEzQ4EUg3mQ
	
	- Additional details on how to setup Vitis for Alveo U200 emulation: https://xilinx.github.io/xup_compute_acceleration/setup_local_computer.html
