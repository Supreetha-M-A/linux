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

### Output
```
[  356.957285] CMPE 283 Assignment 1 Module Start
[  356.957290] Pinbased Controls MSR: 0x3f00000016
[  356.957291]   External Interrupt Exiting: Can set=Yes, Can clear=Yes
[  356.957292]   NMI Exiting: Can set=Yes, Can clear=Yes
[  356.957293]   Virtual NMIs: Can set=Yes, Can clear=Yes
[  356.957294]   Activate VMX Preemption Timer: Can set=No, Can clear=Yes
[  356.957295]   Process Posted Interrupts: Can set=No, Can clear=Yes
[ 3521.084057] CMPE 283 Assignment 1 Module Exits
[ 3718.508189] CMPE 283 Assignment 1 Module Start
[ 3718.508208] Pinbased Controls MSR: 0x3f00000016
[ 3718.508210]   External Interrupt Exiting: Can set=Yes, Can clear=Yes
[ 3718.508212]   NMI Exiting: Can set=Yes, Can clear=Yes
[ 3718.508213]   Virtual NMIs: Can set=Yes, Can clear=Yes
[ 3718.508214]   Activate VMX Preemption Timer: Can set=No, Can clear=Yes
[ 3718.508215]   Process Posted Interrupts: Can set=No, Can clear=Yes
[ 3718.508217] Procbased Controls MSR: 0xf7b9fffe0401e172
[ 3718.508218]   INTERRUPT_WINDOW_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508219]   USE_TSC_OFFSETTING: Can set=Yes, Can clear=Yes
[ 3718.508220]   HLT_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508221]   INVLPG_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508222]   MWAIT_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508223]   RDPMC_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508224]   RDTSC_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508225]   CR3_LOAD_EXITING: Can set=Yes, Can clear=No
[ 3718.508226]   CR3_STORE_EXITING: Can set=Yes, Can clear=No
[ 3718.508227]   CR8_LOAD_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508227]   CR8_STORE_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508229]   USE_TPR_SHADOW: Can set=Yes, Can clear=Yes
[ 3718.508230]   NMI_WINDOW_EXITING: Can set=No, Can clear=Yes
[ 3718.508230]   MOV_DR_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508231]   UNCONDITIONAL_IO_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508232]   USE_IO_BITMAPS: Can set=Yes, Can clear=Yes
[ 3718.508233]   MONITOR_TRAP_FLAG: Can set=No, Can clear=Yes
[ 3718.508233]   USE_MSR_BITMAPS: Can set=Yes, Can clear=Yes
[ 3718.508234]   MONITOR_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508235]   PAUSE_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508236]   ACTIVATE_SECONDARY_CONTROLS: Can set=Yes, Can clear=Yes
[ 3718.508238] Procbased Secondary Controls MSR: 0x51ff00000000
[ 3718.508239]   VIRTUALIZE_APIC_ACCESSES: Can set=Yes, Can clear=Yes
[ 3718.508240]   ENABLE_EPT: Can set=Yes, Can clear=Yes
[ 3718.508240]   DESCRIPTOR_TABLE_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508241]   ENABLE_RDTSCP: Can set=Yes, Can clear=Yes
[ 3718.508242]   VIRTUALIZE_X2APIC_MODE: Can set=Yes, Can clear=Yes
[ 3718.508243]   ENABLE_VPID: Can set=Yes, Can clear=Yes
[ 3718.508243]   WBINVD_EXITING: Can set=Yes, Can clear=Yes
[ 3718.508244]   UNRESTRICTED_GUEST: Can set=Yes, Can clear=Yes
[ 3718.508245]   APIC_REGISTER_VIRTUALIZATION: Can set=Yes, Can clear=Yes
[ 3718.508245]   VIRTUAL_INTERRUPT_DELIVERY: Can set=No, Can clear=Yes
[ 3718.508246]   PAUSE_LOOP_EXITING: Can set=No, Can clear=Yes
[ 3718.508247]   RDRAND_EXITING: Can set=No, Can clear=Yes
[ 3718.508247]   ENABLE_INVPCID: Can set=Yes, Can clear=Yes
[ 3718.508248]   ENABLE_VM_FUNCTIONS: Can set=No, Can clear=Yes
[ 3718.508249]   VMCS_SHADOWING: Can set=Yes, Can clear=Yes
[ 3718.508249]   ENABLE_ENCLS_EXITING: Can set=No, Can clear=Yes
[ 3718.508250]   RDSEED_EXITING: Can set=No, Can clear=Yes
[ 3718.508251]   ENABLE_PML: Can set=No, Can clear=Yes
[ 3718.508252]   EPT_VIOLATION_#VE: Can set=No, Can clear=Yes
[ 3718.508252]   CONCEAL_VMX_FROM_PT: Can set=No, Can clear=Yes
[ 3718.508253]   ENABLE_XSAVES/XRSTORS: Can set=No, Can clear=Yes
[ 3718.508254]   MODE_BASED_EXECUTE_CONTROL_FOR_EPT: Can set=No, Can clear=Yes
[ 3718.508255]   SUB_PAGE_WRITE_PERMISSIONS_FOR_EPT: Can set=No, Can clear=Yes
[ 3718.508255]   INTEL_PT_USES_GUEST_PHYSICAL_ADDRESSES: Can set=No, Can clear=Yes
[ 3718.508256]   USE_TSC_SCALING: Can set=No, Can clear=Yes
[ 3718.508257]   ENABLE_USER_WAIT_AND_PAUSE: Can set=No, Can clear=Yes
[ 3718.508258]   ENABLE_ENCLV_EXITING: Can set=No, Can clear=Yes
[ 3718.508260] Exit Controls MSR: 0x3fefff00036dff
[ 3718.508261]   SAVE_DEBUG_CONTROLS: Can set=Yes, Can clear=No
[ 3718.508262]   HOST_ADDRESS_SAPCE_SIZE: Can set=Yes, Can clear=Yes
[ 3718.508263]   LOAD_IA32_PERF_GLOBAL_CTRL: Can set=No, Can clear=Yes
[ 3718.508263]   ACKNOWLEDGE_INTERRUPT_ON_EXIT: Can set=Yes, Can clear=Yes
[ 3718.508264]   SAVE_IA32_PAT: Can set=Yes, Can clear=Yes
[ 3718.508265]   LOAD_IA32_PAT: Can set=Yes, Can clear=Yes
[ 3718.508266]   SAVE_IA32_EFER: Can set=Yes, Can clear=Yes
[ 3718.508266]   LOAD_IA32_EFER: Can set=Yes, Can clear=Yes
[ 3718.508267]   SAVE_VMX_PREEMPTION_TIMER_VALUE: Can set=No, Can clear=Yes
[ 3718.508268]   CLEAR_IA32_BNDCFGS: Can set=No, Can clear=Yes
[ 3718.508269]   CONCEAL_VMX_FROM_PT: Can set=No, Can clear=Yes
[ 3718.508269]   CLEAR_IA32_RTIT_CTL: Can set=No, Can clear=Yes
[ 3718.508270]   LOAD_CET_STATE: Can set=No, Can clear=Yes
[ 3718.508271]   LOAD_PKRS: Can set=No, Can clear=Yes
[ 3718.508273] Entry Controls MSR: 0xd3ff000011ff
[ 3718.508274]   LOAD_DEBUG_CONTROLS: Can set=Yes, Can clear=No
[ 3718.508275]   IA_32E_MODE_GUEST: Can set=Yes, Can clear=Yes
[ 3718.508276]   ENTRY_TO_SMM: Can set=No, Can clear=Yes
[ 3718.508277]   DEACTIVATE_DUAL_MONITOR_TREATMENT: Can set=No, Can clear=Yes
[ 3718.508277]   LOAD_IA32_PERF_GLOBA_L_CTRL: Can set=No, Can clear=Yes
[ 3718.508278]   LOAD_IA32_PAT: Can set=Yes, Can clear=Yes
[ 3718.508279]   LOAD_IA32_EFER: Can set=Yes, Can clear=Yes
[ 3718.508279]   LOAD_IA32_BNDCFGS: Can set=No, Can clear=Yes
[ 3718.508280]   CONCEAL_VMX_FROM_PT: Can set=No, Can clear=Yes
[ 3718.508281]   LOAD_IA32_RTIT_CTL: Can set=No, Can clear=Yes
[ 3718.508281]   LOAD_CET_STATE: Can set=No, Can clear=Yes
[ 3718.508282]   LOAD_PKRS: Can set=No, Can clear=Yes
```
