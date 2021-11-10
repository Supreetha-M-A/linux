# CMPE283 : Virtualization 

##  Assignment 1: Discovering VMX Features

##### By: Supreetha M A (ID: 015967003), Satish K (ID: 015351284)

#### Questions
##### 1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.
1. Work done by Supreetha M A :
  - Created gcp VM instance from nested VM image. Which formed the base platform to perform the rest of the operations.
  - Downloaded and built Linux source code kernel modules and associated libraries, to create a local copy of linux Kernel.
  - Modified cmpe283-1.c file to add custom logic to print the capabilites of the various MSRs listed in the assignment doc. Referred SDM Volume 3 for the same.
  - Added to stage and commited the Makefile and cmpe283-1.c files after the MSR capabilities output was read from the kernel buffer.
2. Work done by Satish K :
- Tested the machine to verify virtualisation capabilities and features.
- Reasearched on the SDM chapter which provided the structure for each MSR.
- Verified the output of the capabilities listed with the sample output provided in the assignment document.
- Compiled down the steps performed in the process to answer the questions.

##### 2. Describe in detail the steps you used to complete the assignment. 
1. Create gcp VM instance from nested VM image. And ssh into the ubuntu instance
2. Confirm that nested virtualization is enabled.
 run: ``` grep -cw vmx /proc/cpuinfo ```
A nonzero response confirms that nested virtualization is enabled.
3. ```git clone https://github.com/Supreetha-M-A/linux.git```. The repo which is a fork of [Linux source code repo](https://github.com/torvalds/linux)
4. Download the source files Makefile and cmpe283-1.c
5. run ``` make ``` outside the cloned repo and in the place where make file and c file are located.
6. You will get some error which is expected.
7. Enter the linux folder ```cd linux```
8. Run ```uname -a ``` to check what version of linux kernel config is present.
9. Copy the config file contents to a .config file 
```sudo cp /boot/config5.<version> .config```
10. Run ```make oldconfig```. Install gcc, flex and bison when corresponing errors are encountered.
11. Run ```make prepare```. Install libssl-dev and libelf-dev when corresponing errors are encountered.
12. Run ```make -j 4 modules``` (4 as we had 4vcpus) 
13. Run ```make -j 4```
gives error No rule to make target 'debian/canonical-certs.pem', needed by 'certs/x509_certificate_list'. Stop -> disable system trusted keys by running
```scripts/config --disable SYSTEM_TRUSTED_KEYS```
14. Run ```sudo make INSTALL_MOD_STRIP=1 modules_install```
packages the modules built in make modules to a proper location to boot.
15. Run ```sudo make install```.
16. Run ```uname -a``` to see the linux kernel version.
17. Restart the kernel by running  ```sudo reboot```.
18. Run ```uname -a``` to see the kernel version is now the latest(as per the cloned linux repo).
19. Running  ```make```gives error as missing module license.Add license in the cmpe283-1.c file by entering : ```MODULE_LICENSE("GPL V2");```.
20. After which ```make``` succeeds and we see a cmpe283-1.ko file (which is a kernel object)
21. Run ```sudo insmod cmpe283-1.ko``` nothing prints but running ```lsmod | grep cmpe283 ```
we see a timestamp which confirms it was run. But the message was not printed in console, it will be printed in system message buffer.
22. To see the message printed run ```dmesg```.
23. ```sudo rmmod cmpe283-1``` removes the module from kernel.Running ```dmesg``` again will show
exit message.

