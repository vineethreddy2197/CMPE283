<h1>CMPE 283 Assignments</h1>
<h3>By Vineeth Reddy</h3>

<h2>Assignment-02-Instrumentation via hypercall</h2>

<h3>Work done (015363556):</h3>
We began working on the assignment 02 once our assignment 01 environment was built successfully. We decided to edit the cpuid.c and vmx.c at first and then rebuild the kernel. I edited the cpuid.c and added if..else condition in the kvm_emulate_cpuid block code and commited it to the GitHub repo inside /arch/x86/kvm. After Sumeet commited the changes he made to vmx.c, I rebuilt the kernel and rebooted the VM. I also installed KVM on the hypervisor so that a guest VM can be created and test script can be executed on that.  <br>
  
<h3>Steps followed:</h3>
  
1. Update the files arch/x86/kvm/vmx/vmx.c and arch/x86/kvm/cpuid.c <br>
2. Rebuild the kernel using ```make modules``` command and ```make INSTALL_MOD_STRIP=1 modules_install && make install```.<br>
3. Run ```lsmod | grep kvm``` to check if the kvm modules are preloaded.<br>
4. If they are already present remove them using ```rmmod kvm``` and ```rmmod kvm_intel``` commands.<br>
5. Run ```modprobe kvm``` and ```modprobe kvm_intel``` commands to reload edited kvm modules.<br>
  
6. Optional- We installed GUI for out Ubuntu host for a friendly UI and ease fo work. Also, we wanted to use Virtual machine manager to created the nested VM. GUI can be enabled on Ubuntu VM using below commands:(<a href="https://subscription.packtpub.com/book/big-data-and-business-intelligence/9781788474221/1/ch01lvl1sec15/installing-and-configuring-ubuntu-desktop-for-google-cloud-platform">ref</a>)<br>
  ```$ sudo apt-get install gnome-shell``` <br>
  ```$ sudo apt-get install ubuntu-gnome-desktop``` <br>
  ```$ sudo apt-get install autocutsel``` <br>
  ```$ sudo apt-get install gnome-core``` <br>
  ```$ sudo apt-get install gnome-panel``` <br>
  ```$ sudo apt-get install gnome-themes-standard``` <br> 
  
7. Install xrdp through the below commands- we need this to be able to access Ubuntu in GUI mode.<br>
  ```sudo apt-get update``` <br>
  ```sudo apt-get install -y xrdp``` <br>
  ```sudo apt-get install -y xfce4``` <br>
  ```sudo service xrdp restart``` <br>

8. Login to the host VM using RDP to have graphical interface.<br> 
9. To enable the kvm module in the host and install necessary packages, run the below commands from host terminal: (<a href="https://www.tecmint.com/install-kvm-on-ubuntu/">ref</a>) <br>
  ```sudo apt install qemu qemu-kvm qemu-system qemu-utils``` <br>
  ```sudo apt install libvirt-clients libvirt-daemon-system virtinst``` 
10. Run Virtual Machine Manager and create a new VM inside the host. (download the iso file or guest VM as a prerequisite)<br>
11. Install Guest OS once the VM is created and login to the nested VM.<br>
12. Install CPUID using ``` sudo apt install cpuid ``` if it is an Ubuntu VM <br>  
13. Run the command ```cpuid -l 0x4FFFFFFF``` to verify the output.<br>
14. Run the test bash script to produce results and print number of exits.<br>
15. Run the test2 bash script to produce number of cycles in ebx and ecx registers when eax=0x4ffffffe.<br>



<h3>Output Screenshots:</h3>
<ul>
<li>Output screen that verifies that kvm is installed on Ubuntu host.<br>
  
  ![image](https://user-images.githubusercontent.com/89494219/142976711-117f65f3-75ad-407e-9132-e9dd0c094fd6.png)
<br>
  
<li>Output screen that shows nested VM created on KVM Host:<br>
  
  ![image](https://user-images.githubusercontent.com/89494219/142977493-ea58632d-4b90-4836-a7da-311dea1c3184.png)
  
  <br>

  ![image](https://user-images.githubusercontent.com/89494219/142977561-3eb6f60e-07d5-4b62-9270-ba2a0dd50086.png)
  
  <br>
  
  <li>Output screen that shows number of exits when eax=0x4fffffff:<br>

  ![image](https://user-images.githubusercontent.com/89494219/142977843-5bcd7169-33d6-41da-9c53-4a7ae8dca34f.png)

  <br>
    
  <li>Output screen that shows cycles spent when eax=0x4ffffffe:<br>
    
 ![image](https://user-images.githubusercontent.com/89494219/142978056-a00ec5ca-aaa8-44fa-9b59-e6bfc8a793ae.png)
 <br>

    
<h1>Assignment-03</h1>
    
<h3>Work done by Vineeth:</h3>
I edited the cpuid.c code block for eax=0x4ffffffd to return the number of exits for a specific exit reason and return the value in eax register. I then ran the make command again to verify output through cpuid command in neested VM and dmesg command in the host VM.
I edited the cpuid.c code block for eax=0x4ffffffc to return the time spent processing the exit number provided in ecx and return the high 32 bits in %ebx and low 32-bit in %ecx. I then commited this code change to the GitHub folder CMPE-283-Assignment-3.
      
    
<h3>Steps followed:</h3>
    
1. Run the assignment-2 environment. <br>
    
2. Navigate to ~/linux/arch/x86/kvm/cpuid.c and edit the code block. Put another if..else condition for when eax = 0x4ffffffd. <br>
	  
	  	else if(eax == 0x4ffffffd) 
        {

		//reasons not in SDM
		if(ecx==35 || ecx==38 || ecx==42 || ecx==65 || ecx>68 || ecx<0){
			printk(KERN_INFO "exit reason number = %u not defined by SDM",ecx);
			eax=0;
			ebx=0;
			ecx=0;
			edx=0xffffffff;
		}
		else if( ecx==5 || ecx==6 || ecx==11 || ecx==17 ||  ecx==35 || ecx==38 || ecx==42 || ecx==66){
				printk(KERN_INFO"exit reason number =%u not enabled in KVM",ecx);
				eax=ebx=ecx=edx=0;
			}
		else{
				printk(KERN_INFO "CPUID(0x4ffffffd), exit number=%u exits=%d\n",ecx,arch_atomic_read(&exitsPerReason[ecx]));
				eax=atomic_read(&exitsPerReason[(int)ecx]);
				ebx=ecx=edx=0;
			}
		} 
     
3. Make the necessary changes in vmx.c as well (variable declarations)<br>
4. Make changes for code block and write if..else condition for when eax = 0x4ffffffc. <br>
	  
    		else if(eax == 0x4ffffffc)
        {

		if(ecx==35 || ecx==38 || ecx==42 || ecx==65 || ecx>68 || ecx<0){
                        printk(KERN_INFO "exit reason number = %u not defined by SDM",ecx);
                        eax=0;
                        ebx=0;
                        ecx=0;
                        edx=0xffffffff;
                }
                else if( ecx==5 || ecx==6 || ecx==11 || ecx==17 ||  ecx==35 || ecx==38 || ecx==42 || ecx==66){
                                printk(KERN_INFO"exit reason number =%u not enabled in KVM",ecx);
                                eax=ebx=ecx=edx=0;
		}
                                                                     
5. Save the changes and run the below commands in order as mentioned.<br>
    ``` sudo make -j 16 modules ``` <br>
    ``` sudo make -j 16 ```                                                             
    ``` sudo make INSTALL_MOD_STRIP=1 modules_install ```                                                          
    ``` rmmod kvm_intel ```<br>
    ``` rmmod kvm ```<br>
    ``` modprobe kvm_intel ``` <br>
    ``` modprobe kvm ``` <br>
                                                                     
6. Now run the nested VM and run test script or ``` cpuid -l 0x4ffffffd -s <exit reason> ``` to verify output for different exit reasons.<br>   
    
7. Run dmesg in the host VM to get output. <br>  
	 
<h3>Answer to Questions:</h3>
	  <h4>Question-3</h4>
	  
We noticed that the count increased at a stable rate. Below are the screenshots for exit reason=0. The exit count is currently 12035. <br>
	  
![image](https://user-images.githubusercontent.com/89494219/143815197-e62e473b-7783-4ce0-a903-e9e255ebca9e.png) <br>
		  
We did another reboot to see the increase in count of exits. The exit count raised to 24070 which is exactly the double of what it was previously. <br>
		  
 ![image](https://user-images.githubusercontent.com/89494219/143815337-7e50ea14-6c10-45bb-b619-099a0b0c1312.png) <br>
	  
We did one more reboot to find tha rate of increase. The number of exits is now 36105. Which is three times of the first output. Which conculdes that the number of exits is increasing at a stable rate.<br>

![image](https://user-images.githubusercontent.com/89494219/143815645-a9ed64b6-db66-4d8d-9140-d2d761613205.png) <br>
	  
For exit reason=0, the number of exits increase by approximately 12k on each boot.<br>	  
                           
<h4>Question-4</h4>
	  
The most frequent exits were noticed for exit reason =48.<br>
	  
![image](https://user-images.githubusercontent.com/89494219/143813811-a4406c3c-54da-4f83-b9da-c1ab2f9f9987.png) <br>

There were many exit reasons with 0 exits (least frequent). The full dmesg output is in the test3.txt /CMPE-283-Assignment-3 folder.	  <br>
	
![image](https://user-images.githubusercontent.com/89494219/143813909-fce3b21a-3902-4c67-9574-5b538c7c5ace.png) <br>

<h4>Other Output Screenshots </h4>

<li> Output of cpuid command for ebx(high 32-bit) and ecx(low 32-bit) values when eax=0x4ffffffc. <br>
  
![image](https://user-images.githubusercontent.com/89494219/143814645-9ebb21ad-a3d2-4dfa-a304-9d19ceb07f7b.png) <br>


<li> dmesg output of test4 script for ebx(high 32-bit) and ecx(low 32-bit) values when eax=0x4ffffffc. Full dmesg logs is in the test4.txt file in CMPE-283-Assignment-3 folder.<br>
  
![image](https://user-images.githubusercontent.com/89494219/143814514-400258f3-5435-4c8f-8cd3-07316804589a.png) <br>


<h1>Assignment-04</h1>

<h3>Work done by Vineeth:</h3>
We started working on GCP instance on which Assignment 3 was finished. I worked on performance when using shadow paging to illustrate the different exit frequencies and types. I removed the kvm-intel module and reloaded with parameter ept=0 and recorded the total exit count information in shadow.txt file.
We started working on GCP instance on which Assignment 3 was finished. I worked on performance when using nested paging to illustrate the different exit frequencies and types. I recorded the total exit count information in nested.txt file.
	
<h3>Question 2: Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.</h3>

<h4> Output when EPT=0 (Shadow Paging)</h4>
	
![image](https://user-images.githubusercontent.com/89494219/145184480-7823eb65-b6c2-4fb7-9111-a8f4dbf30c58.png) <br>

	

	
<h4> Output when EPT = non zero (Nested Paging)</h4>
	
![image](https://user-images.githubusercontent.com/89494219/145183459-22cf0bae-b61f-4234-b512-b07f6b3ff123.png) <br>

	
<h3>Question 3: What did you learn from the count of exits? Was the count what you expected? If not, why not?</h3>

Exit Code | Nested Paging | Shadow Paging
| :---: | :---: | :---:
0 | 12035 | 857331
10 | 138268 | 280938
14 | 0 | 101493
28 | 25697 | 5732292
29 | 2 | 4
30 | 148345 | 295253
31 | 1531 | 3306
46 | 6 | 12
47 | 2 | 4
48 | 383853 | 383853
54 | 3 | 7
55 | 3 | 6
58 | 0 | 65707

As per above observations, the number of exits change for both nested paging and shadow paging. With ept=0 which is shadow paging, the exit counts of exit reason 0, 10, 14, 28, 58 amplifies. This is because there are overheads associated when shadow paging is enabled. It is noticed that the exit code 28 occurs far more frequently in shadow paging mode as compared to nested paging.
	

<h3>Question 4: What changed between the two runs (ept vs no-ept)?</h3>

While comparing the values of total exit count with ept and without ept, we saw that when the parameter ept=0 is changed, the exit counts for few exit numbers significantly increases. The reason being that shadow paging uses a two-layer translation from guest physical address to host physical address and it involved more VMM involvement which causes additional VM exits. The shadow paging maintains a extra shadow page table which requires extra memory. On the other hand, for nested paging there are two page directories- one to guest virtual addr. to guest physical addr. and anothet to map guest physical addr. to host physical addr. This requires no intervention from VMM, and results in less VM Exits.
