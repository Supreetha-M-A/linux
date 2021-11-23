# CMPE283 : Virtualization 

##  Assignment 2: Instrumentation via hypercall

##### By: Supreetha M A (ID: 015967003), Satish K (ID: 015351284)

### Questions

### Q-1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.
##### Work done by Satish K:  
1. Created leaf node %eax= 0x4FFFFFFF for case I
2. Made required changes in cpuid.c and vmx.c
3. Installed cpuid Pakage on inner VM
4. Tested and Verified results
5. Documented the steps and results.
 
##### Work done by Supreetha M A:  

1. Created leaf node %eax= 0x4FFFFFFD for case III 
2. Made required changes in cpuid.c and vmx.c 
3. Tested and Verified results
4. Documented the steps and results.

### Q-2. Describe in detail the steps you used to complete the assignment. 
#### Prerequisites:  
Need a working assignment 1 configuration.
 
#### Verified: 
Assignment I code is functional.

### Step 1: Add code to KVM at file /linux/arch/x86/kvm/vmx/vmx.c and /linux/arch/x86/kvm/cpuid.c

1. For CPUID leaf node %eax= 0x4FFFFFFF: 
Return the total number of exits (all types) in %eax.
2. For CPUID leaf node %eax= 0x4FFFFFFD:
Return the number of exits for the exit number provided (on input) in %ecx. The output should be returned in %eax.

### Step 2: Build the updated code:  
```
make modules
make modules_install
make install
Reboot
```	

### Step 3: Open virt-manager and start virtual machine. Install CPUID package inside the inner vm. 
```sudo apt-get install cupid```
 
### Step 4: Test the code using below commands for case 1:
1. cpuid -l 0x4FFFFFFF
2. Try dmesg command in the host system’s terminal
Total exits taken for VM reboot: 677887

### Step 5: Test the code using below commands for case 2:
 
1. cpuid -l 0x4FFFFFFD s -12 (Exit 12)
2. Try dmesg command in the host system’s terminal to view count of all available exits.
3. Cupid -l 0x4FFFFFFD s -444 to test the output for invalid exit code.
4. cupid -l 0x4FFFFFFD s -3 to test the output for valid exit but not implemented by KVM.

### Q-3: Comment of the frequency of exits
### Answer: 
Frequency of the exits are dependent on use of the system. If the system performs the more privileged operations, then the number of exits increases.


### Q-4: Exit types defined in SDM, which are the most frequent and least frequent?
### Answer: 
 Most frequent exit is MSR_WRITE with count 605825
 Least frequent exit is DR_ACCESS with count 8 (non-zero)

