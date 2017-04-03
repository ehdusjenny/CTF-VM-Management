# How to Manage CTF VMs

Hypervisors:
* ssh hobbes.sus.mcgill.ca
* ssh calvin.sus.mcgill.ca


Proxmox GUI: 
* https://hobbes.sus.mcgill.ca:8006
* https://calvin.sus.mcgill.ca:8006

You need access to:
* Above two GUI
* dhcp.sus.mcgill.ca
* rose.sus.mcgill.ca

To create an account to access Proxmox, ask the server admin to create an account on hobbes.sus.mcgill.ca by:
	1. ssh hobbes.sus.mcgill.ca
	2. `sudo adduser whatever-user`
	3. On GUI, go to the **Users** tab
	4. **Add** user, and remember to specify the first and last name
	5. Go to the **Permissions** tab. Give the new user **User Permission**.
	6. Path should be /, checkmark propagate, and give the minimum Role the user needs.
User should be able to log in to GUI at this point.

How to Create VM 
1. Download Debian netinstall iso (newest version)
	* Choose amd64 
	* https://www.debian.org/CD/netinst/ 
 	![Alt](images/1.png)
1. Go to the GUI, click **local** storage -> **Content** tab -> **Upload** if the latest version is not already there.
	![Alt](images/2.png)
	![Alt](images/3.png)
	![Alt](images/4.png)
	![Alt](images/5.png)
1. Click **Create VM**
	![Alt](images/5_1.png)
1. General: Set VM ID and name -> Next
	![Alt](images/5_2.png)
1. OS: Choose **Linux 3.X/2.6 Kernel (l26)** -> Next
	![Alt](images/5_3.png)
1. CD/DVD: Use CD/DVD disc image file (iso) with local storage and the latest Debian ISO
	![Alt](images/5_4.png)
1. Hard Disk: Local storage -> change disk size as needed -> QEMU format -> Next
	![Alt](images/5_5.png)
1. Change CPU setting as needed -> Next
	![Alt](images/5_6.png)
1. Change Memory size as needed -> Next
	![Alt](images/5_7.png)
1. Leave Network as is -> Next
	![Alt](images/5_8.png)
1. Confirm: Finish
	![Alt](images/5_9.png)
1. Start the VM by right clicking on the VM and clicking **Start**
	![Alt](images/5_10.png)
1. If the console is not working, use Firefox (At the time of this guide update, Chrome does not support Java sadly)
	* Go to **Java Control Panel** by searching Java in the start menu.
	![Alt](images/5_11.png)
	* Go to **Security** tab and click **Edit Site List...**
	![Alt](images/5_12.png)
	* Add https://hobbes.sus.mcgill.ca and https://hobbes.sus.mcgill.ca:8006 to the list and press OK
	![Alt](images/5_13.png)
	*Wait several minutes and open the console again. If console fails, open a new one. The second one should work.
	* IF above fails, use VNC Viewer
1. From console, **Install** -> **English** -> **Canada** -> **American English** (Keyboard)
	![Alt](images/6.png)
	![Alt](images/7.png)
	![Alt](images/8.png)
	![Alt](images/9.png)
1. Your hostname should be in the format **your-new-vm.science.mcgill.ca**.
1. Ask server admin for the root password to be used.
1. Create your personal non-admin account.
1. **Eastern** (Time Zone)
	![Alt](images/10.png)
1. Partition disk to use **entire disk**
	* Choose different partitioning schemes depending on your preference
	![Alt](images/11.png)
	![Alt](images/12.png)
	![Alt](images/13.png)
	![Alt](images/14.png)
	![Alt](images/15.png)
1. To configure the Package Manager, choose **Canada** -> **ftp.ca.debian.org**
	![Alt](images/16.png)
	![Alt](images/17.png)
1. You shouldn't need to use a HTTP proxy so leave the field blank
	![Alt](images/18.png)
1. Choose the right software based on your needs.
	* **Recommendation**: Uncheck Debian desktop environment and check SSH server
1. This should be the only OS on the VM so install the GRUB boot loader to the master boot record at **/dev/sda**.
	![Alt](images/21.png)
	![Alt](images/22.png)
1. Yay! You're done the OS installation. Restart the VM and terminal should appear for you to login using your personal account.
	![Alt](images/23.png)
1. Now we need to configure network. Get the **MAC address** under Hardware tab -> double click Network Device -> copy MAC Address.
	![Alt](images/24.png)
	![Alt](images/25.png)
1. ssh to dhcp.sus.mcgill.ca. If you do not have access or sudo access, ask your server admin.
1. Run `sudo vim /etc/dhcp/hosts.mcconnell`.
1. Add your new vm to the end of the list in the following format:
    * host your-new-vm {
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; hardware ethernet MAC ADDRESS;
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; fixed-address 132.216.25.<unused number>;
    <br>
    }
    * Just to double check, ping the IP and make sure it is not being used
1. Now if you want to set up DNS, go to https://calvin.sus.mcgill.ca:8006 and go to rose server's console.
    * rose.sus.mcgill.ca and howard.sus.mcgill.ca are our DNS servers
    * rose is the master server, howard is the slave server. When bind9 is restarted, howard will replicate rose.
1. Run `cd /etc/bind/zones`
1. Run `vim db.science.mcgill.ca`
1. Add a new A record `your-new-vm IN A your-IP`
1. Update serial number with the current date YYYYMMDD
1. Run `sudo service bind9 restart`
1. ssh to dhcp.sus.mcgill.ca and run `sudo service isc-dhcp-server restart`
    * Give some time for DNS to propagate. After 5-10 minutes, you should be able to ssh to your-new-vm.science.mcgill.ca.



Separate Tutorial for Setting Up JetBrains License Server
1.
1.
